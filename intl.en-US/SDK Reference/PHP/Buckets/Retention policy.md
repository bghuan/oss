# Retention policy

You can configure a time-based retention policy for a bucket. A retention policy has a retention period that ranges from one day to 70 years. This topic describes how to create, query, and lock a retention policy.

## Background information

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time.

If a retention policy is not locked within 24 hours after it is created, the retention policy becomes invalid. After the retention policy configured for a bucket is locked, you can upload objects to or read objects from the bucket. However, you cannot delete objects in the bucket or the retention policy within the retention period of the policy. The retention period of the policy cannot be shortened but only be extended. For more information about retention policies, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

## Create a retention policy

The following code provides an example on how to create a retention policy:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Create a retention policy and set the retention period to 30 days.
    $wormId = $ossClient->initiateBucketWorm($bucket, 30);

    // Query the ID of the retention policy.
    print($wormId);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n"); 
```

For more information about how to create a retention policy, see [InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md).

## Cancel an unlocked retention policy

The following code provides an example on how to cancel an unlocked retention policy:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Cancel the retention policy.
    $ossClient->abortBucketWorm($bucket);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n"); 
```

For more information about how to cancel an unlocked retention policy, see [AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md).

## Lock a retention policy

The following code provides an example on how to lock a retention policy:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Create a retention policy and set the retention period to 30 days.
    $wormId = $ossClient->initiateBucketWorm($bucket, 30);

    // Lock the retention policy.
    $ossClient->completeBucketWorm($bucket, $wormId);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");   
```

For more information about how to lock a retention policy, see [CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md).

## Query a retention policy

The following code provides an example on how to query a retention policy:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Query the retention policy.
    $config = $ossClient->getBucketWorm($bucket);

    // Display the information about the retention policy.
    printf("WormId:%s\n", $config->getWormId());
    printf("State:%s\n", $config->getState());
    printf("Day:%d\n", $config->getDay());
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");    
```

For more information about how to query a retention policy, see [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md).

## Extend the retention period of a retention policy

The following code provides an example on how to extend the retention period of a locked retention policy:

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
$wormId = "<yourWormId>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Extend the retention period of the locked retention policy to 120 days.
    $ossClient->ExtendBucketWorm($bucket, $wormId, 120);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to extend the retention period of a locked retention policy, see [ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md).

