# Query object metadata

By default, when you perform the HeadObject operation on an object in a versioned bucket, only the metadata of the current version of the object is returned.

**Note:** If the current version of the object is a delete marker, OSS returns 404 Not Found. If you specify a version ID in the request, OSS returns the metadata of the object with the specified version ID.

The following code provides an example on how to query object metadata:

```
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Query all metadata of the object with the specified version ID.
    $objectMeta = $ossClient->getObjectMeta($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));
    print_r($objectMeta);

    // Query part of metadata of the object with the specified version ID.
    $objectMeta = $ossClient->getSimplifiedObjectMeta($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));
    print_r($objectMeta);
  
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to query object metadata, see [GetObjectMeta](/intl.en-US/API Reference/Object operations/Basic operations/GetObjectMeta.md).

