# Restore objects

This topic describes how to restore objects of the Archive and Cold Archive storage classes.

**Note:** You must restore an archived or cold archived object before you can read it. Do not call RestoreObject to perform restore operations on objects of which the storage class is not Archive and Cold Archive. For more information about the Archive and Cold Archive storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about related status codes, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) in API Reference.

## Restore archived objects

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore an archived object:

```
#include <alibabacloud/oss/OssClient.h>
#include <thread>
#include <chrono>
using namespace AlibabaCloud::OSS;

int main(void)
{

    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Restore the archived object. */
    auto outcome = client.RestoreObject(BucketName, ObjectName);

    /* Objects of which the storage class is not Archive and Cold Archive cannot be restored. */
    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "RestoreObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    std::string onGoingRestore("ongoing-request=\"false\"");

    int maxWaitTimeInSeconds = 600;
    while (maxWaitTimeInSeconds > 0)
    {
        auto meta = client.HeadObject(BucketName, ObjectName);

        std::string restoreStatus = meta.result().HttpMetaData()["x-oss-restore"];
        std::transform(restoreStatus.begin(), restoreStatus.end(), restoreStatus.begin(), ::tolower);
        if (! restoreStatus.empty() && 
        restoreStatus.compare(0, onGoingRestore.size(), onGoingRestore)==0) {
            std::cout << " success, restore status:" << restoreStatus << std::endl;
            /* The archived object is restored. */
            break;
        }

        std::cout << " info, WaitTime:" << maxWaitTimeInSeconds
        << "; restore status:" << restoreStatus << std::endl;

        std::this_thread::sleep_for(std::chrono::seconds(10));
        maxWaitTimeInSeconds--;     
    }

    if (maxWaitTimeInSeconds == 0)
    {
        std::cout << "RestoreObject fail, TimeoutException" << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Restore cold archived objects

The status of a cold archived object throughout the restoration process is described as follows:

1.  By default, the cold archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore a cold archived object:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Refer to the following code if you set the storage class of the object to upload to Cold Archive. */
    //auto content = std::make_shared<std::stringstream>("test");
    //PutObjectRequest putRequest(BucketName, ObjectName, content);
    //putRequest.MetaData().addHeader("x-oss-storage-class", "ColdArchive");
    //auto putOutcome = client.PutObject(putRequest);

    RestoreObjectRequest request(BucketName, ObjectName);
    /* Configure the duration for which the object can remain in the restored state. The default value is one day. */
    request.setDays(2);
    /* Configure the restore mode of the cold archived object. */
    request.setTierType(TierType::Expedited);
    auto outcome = client.RestoreObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "Delete Bucket Inventory fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

