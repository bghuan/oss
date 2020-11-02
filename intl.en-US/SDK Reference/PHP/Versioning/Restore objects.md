# Restore objects

In a versioned bucket, the storage classes of different versions of an object can be different. By default, when you perform the RestoreObject operation on an object, the current version of the object is restored. You can specify a version ID in the request to restore the specified version of an object.

The following code provides an example on how to restore an object:

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
use OSS\Model\DeleteObjectInfo;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
// Specify the complete name of the object excluding the bucket name. Example: example/test.txt.
$object= "<yourObjectName>";
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try{
    // Restore the object with the specified version ID.
    $result = $ossClient->restoreObject($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));

} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to restore an object, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md).

