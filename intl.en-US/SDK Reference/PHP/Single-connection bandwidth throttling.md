# Single-connection bandwidth throttling

This topic describes how to add and configure the x-oss-traffic-limit header in object upload or download requests to limit the upload or download bandwidth. This way, you can ensure other applications have sufficient bandwidth when you upload objects to or download objects from OSS.

**Note:** For more information about the scenarios and usage notes of single-connection bandwidth throttling, see [Single-connection bandwidth throttling](/intl.en-US/Developer Guide/Objects/Single-connection bandwidth throttling.md) in the OSS Developer Guide.

## Configure bandwidth throttling in simple upload and download

The following code provides an example on how to configure bandwidth throttling when you call PutObject to upload an object and call GetObject to download an object:

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
$object= "<yourObjectName>";
$content = "hello world";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

// Set the bandwidth throttling to 100 KB/s, which is equal to 819200 bit/s.
$options = array(
      OssClient::OSS_HEADERS => array(
              OssClient::OSS_TRAFFIC_LIMIT => 819200,
));

try {
    // Configure bandwidth throttling for object upload.
    $ossClient->putObject($bucket, $object, $content, $options);

    // Configure bandwidth throttling for object download.
    $ossClient->getObject($bucket, $object, $options);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

## Configure bandwidth throttling for upload and download that use signed URLs

The following code provides an example on how to configure bandwidth throttling when you use signed URLs to upload or download an object:

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
$object= "<yourObjectName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

// Set the bandwidth throttling to 100 KB/s, which is equal to 819200 bit/s.
$options = array(
        OssClient::OSS_TRAFFIC_LIMIT => 819200,
);

// Generate a signed URL used to upload an object with bandwidth throttling configured and set the validity period of the URL to 60 seconds.
$timeout = 60;
$signedUrl = $ossClient->signUrl($bucket, $object, $timeout, "PUT", $options);
print($signedUrl);

// Generate a signed URL used to download an object with bandwidth throttling configured and set the validity period of the URL to 120 seconds.
$timeout = 120;
$signedUrl = $ossClient->signUrl($bucket, $object, $timeout, "GET", $options);
print($signedUrl);
```

