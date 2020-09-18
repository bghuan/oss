# Restore objects

This topic describes how to restore objects of the Archive and Cold Archive storage classes.

**Note:** You must restore an Archive or Cold Archive object before you can read it. Do not call RestoreObject to perform restore operations on objects of which the storage class is not Archive or Cold Archive.

For more information about the Archive and Cold Archive storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about related status codes, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) in API Reference.

## Restore Archive objects

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

For the complete code used to restore Archive objects, visit [GitHub](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/RestoreArchiveObjectSample.cs).

The following code provides an example on how to restore an archived object:

```
using Aliyun.OSS;
using Aliyun.OSS.Model;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
var objectContent = "More than just cloud.";
int maxWaitTimeInSeconds = 600;
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Create an Archive bucket.
    var bucket = client.CreateBucket(bucketName, StorageClass.Archive);
    Console.WriteLine("Create Archive bucket succeeded, {0} ", bucket.Name);
}
catch (Exception ex)
{
    Console.WriteLine("Create Archive bucket failed, {0}", ex.Message);
}
// Upload an object.
try
{
    byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
    MemoryStream requestContent = new MemoryStream(binaryData);
    client.PutObject(bucketName, objectName, requestContent);
    Console.WriteLine("Put object succeeded, {0}", objectName);
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
var metadata = client.GetObjectMetadata(bucketName, objectName);
string storageClass = metadata.HttpMetadata["x-oss-storage-class"] as string;
if (storageClass ! = "Archive")
{
    Console.WriteLine("StorageClass is {0}", storageClass);
    return;
}
// Restore the object.
RestoreObjectResult result = client.RestoreObject(bucketName, objectName);
Console.WriteLine("RestoreObject result HttpStatusCode : {0}", result.HttpStatusCode);
if (result.HttpStatusCode ! = HttpStatusCode.Accepted)
{
    throw new OssException(result.RequestId + ", " + result.HttpStatusCode + " ,");
}
while (maxWaitTimeInSeconds > 0)
{
    var meta = client.GetObjectMetadata(bucketName, objectName);
    string restoreStatus = meta.HttpMetadata["x-oss-restore"] as string;
    if (restoreStatus ! = null && restoreStatus.StartsWith("ongoing-request=\"false\"", StringComparison.InvariantCultureIgnoreCase))
    {
        break;
    }
    Thread.Sleep(1000);
    maxWaitTimeInSeconds--;
}
if (maxWaitTimeInSeconds == 0)
{
    Console.WriteLine("RestoreObject is timeout. ");
    throw new TimeoutException();
}
else
{
    Console.WriteLine("RestoreObject is successful. ");
}
```

## Restore Cold Archive objects

The status of a Cold Archive object throughout the restoration process is described as follows:

1.  By default, the cold archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore a Cold Archive object:

```
using Aliyun.OSS;
using Aliyun.OSS.Model;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";

// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);

//Restore the Cold Archive object.
var request = new RestoreObjectRequest(bucketName, objectName);

// Configure the restoration priority of the Cold Archive object.
// TierType.Expedited: The object is restored within an hour.
// TierType.Standard: The object is restored within two to five hours.
// TierType.Bulk: The object is restored within five to twelve hours.
// Configure parameters. For example, set the restoration priority of the object to Standard, and set the duration for which the object can remain in the restored state to two days.
// The Days parameter indicates the validity period of the restored state. The default value is one day. This parameter applies to Archive and Cold Archive objects.
// The Tier indicates the restoration priority of the object and applies only to Cold Archive objects.
request.Days = 2;
request.Tier = TierType.Expedited;
RestoreObjectResult result = client.RestoreObject(request);
Console.WriteLine("RestoreObject result HttpStatusCode : {0}", result.HttpStatusCode);
```

