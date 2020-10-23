# Delete the tags added to an object

OSS allows you to configure object tagging to classify objects. This topic describes how to delete the tags added to an object.

The object tagging feature uses a key-value pair to tag an object. For more information about object tags, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md) in OSS Developer Guide.

The following code provides an example on how to delete the tags added to an object:

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
// Specify the complete name of the object from which you want to delete tags, including the object extension. Example: example/test.jpg.
$object= "<yourObjectName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Delete the tags added to the object.
    $ossClient->deleteObjectTagging($bucket, $object);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to delete object tags, see [DeleteObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/DeleteObjectTagging.md).

