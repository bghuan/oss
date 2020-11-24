# Query object metadata

By default, calling the HeadObject operation on an object in a versioning-enabled bucket returns only metadata of the current version of the object.

**Note:** If the current version of the object is a delete marker, OSS returns 404 Not Found. If you specify the version ID in your request, OSS returns metadata of the object with the specified version ID.

The following code provides an example on how to obtain object metadata:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
// Specify the complete name of the object excluding the bucket name. Example: example/test.jpg.
var objectName = "<yourObjectName>";
var versionid = "<yourArchiveObjectVersionid>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Query the object metadata.
    var request = new GetObjectMetadataRequest(bucketName, objectName)
    {
        // Query the metadata of the specified version of the object.
        VersionId = versionid
    };

    // Query all metadata of the specified version of the object.
    var oldMeta = client.GetObjectMetadata(request);

    // Query partial metadata of the specified version of the object.
    var meta = client.GetSimplifiedObjectMetadata(request);
}
catch (Exception ex)
{
    Console.WriteLine("Put object failed, {0}", ex.Message);
}
```

For more information about how to obtain the metadata of an object, see [GetObjectMeta](/intl.en-US/API Reference/Object operations/Basic operations/GetObjectMeta.md) and [HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md).

