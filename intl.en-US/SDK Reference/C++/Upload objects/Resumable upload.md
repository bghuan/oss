# Resumable upload

Before you upload an object to OSS by using resumable upload, you can specify a folder for the checkpoint file that stores resumable upload records. If a file fails to be uploaded because the network is abnormal or the program stops responding, the upload task is resumed from the position recorded in the checkpoint file to upload remaining bytes.

## Background information

During resumable upload, the upload progress is recorded in a checkpoint file. If the upload of a part fails, the next upload starts from the position recorded in the checkpoint file. The checkpoint file is deleted after resumable upload is complete.

You can use client.ResumableUploadObject to perform resumable upload. The following table describes the parameters you can configure for UploadObjectRequest.

|Parameter|Description|Required|Default value|Configuration method|
|:--------|:----------|:-------|:------------|:-------------------|
|bucket|The name of the bucket.|Yes|None|Constructor|
|key|The name of the object.|Yes|None|
|filePath|The name of the local file to upload.|No|None|
|partSize|The size of each part. Valid values: 100 KB to 5 GB.|No|8 MB|setPartSize|
|threadNum|The number of parts you upload simultaneously.|No|3|Constructor or setThreadNum|
|checkpointDir|The checkpoint file that records the upload results of each part. This file stores information about upload progress. If you fail to upload a part, the part upload continues based on the recorded progress. After the entire local file is uploaded, this file is deleted.|No|The same directory as that of DownloadFile|Constructor or setCheckpointDir|

For more information about how to perform resumable upload, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md). For the complete code, visit [GitHub](https://github.com/aliyun/aliyun-oss-cpp-sdk/blob/f7ef0efa45e17da815020a18919551973cc98089/sample/src/object/ObjectSample.cc#L318).

## Sample code

The following code provides an example on how to perform resumable upload by using multipart upload.

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
    std::string UploadFilePath = "yourUploadfilePath";
    std::string CheckpointFilePath = "yourCheckpointFilepath"

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Start resumable upload. */
    UploadObjectRequest request(BucketName, ObjectName, UploadFilePath, CheckpointFilePath);
    auto outcome = client.ResumableUploadObject(request);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "ResumableUploadObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

