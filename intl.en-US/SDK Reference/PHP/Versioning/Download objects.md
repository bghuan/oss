# Download objects

By default, when you perform the GetObject operation on an object in a versioned bucket, only the current version of the object is returned.

When you perform the GetObject operation on an object in a bucket, you may obtain one of the following results:

-   If the current version of the object is a delete marker, OSS returns 404 Not Found.
-   If the version ID of the object is specified in the request, OSS returns the specified version of the object. If the version ID is set to null in the request, OSS returns the version whose version ID is null.
-   If the version ID specified in the request is the version ID of a delete marker, OSS returns 405 Method Not Allowed.

The following code provides an example on how to download an object:

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
// Specify the complete name of the object excluding the bucket name. Example: example/test.txt.
$object = "<yourObjectName>";
// Specify the version ID of the version you want to download.
$versionId = "<yourObjectVersionId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try{
    // Download the version with the specified version ID.
    $content = $ossClient->getObject($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));
    print($content);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");        
```

For more information about how to download an object, see [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md).

