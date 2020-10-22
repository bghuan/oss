# Bucket policy

You can use bucket policies to authorize other users to access your OSS resources. This topic describes how to configure, query, and delete a bucket policy.

## Scenarios

Bucket policies provide resource-based authorization for users and apply to the following scenarios:

-   Grant RAM users of other Alibaba Cloud accounts permissions to access your OSS resources.

    You can grant RAM users of other Alibaba Cloud accounts permissions to access your OSS resources.

-   Grant anonymous users from certain IP addresses permissions to access your OSS resources.

    In some cases, you need to grant anonymous users from a specified IP address permissions to access your OSS resources. For example, you want to grant only users within your enterprise but not users in other regions permissions to access some confidential enterprise documents. In this case, you can configure a bucket policy that allows only access from specified IP addresses to authorize users more efficiently.

    For more information about bucket policy configurations and use cases, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use bucket policies to authorize other users to access OSS resources.md). For more information about the structure of a bucket policy, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).


## Configure bucket policies

The following code provides an example on how to configure a bucket policy:

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

// Specify the bucket policy.
$policy = <<< BBBB
{
  "Version":"1",
  "Statement":[
  {
    "Action":[
    "oss:PutObject",
    "oss:GetObject"
  ],
    "Effect":"Allow",
    "Resource":["acs:oss:*:*:*/user1/*"]
  }
  ]
}
BBBB;

try {
    // Configure the bucket policy.
    $ossClient->putBucketPolicy($bucket, $policy);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to configure bucket policies, see [PutBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/PutBucketPolicy.md).

## Query bucket policies

The following code provides an example on how to query the policies configured for a bucket:

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
    // Query the bucket policies.
    $policy = $ossClient->getBucketPolicy($bucket);

    // Display the bucket policies.
    print($policy);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to query bucket policies, see [GetBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/GetBucketPolicy.md).

## Delete bucket policies

The following code provides an example on how to delete the policies configured for a bucket:

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
    // Delete the policies configured for the bucket.
    $ossClient->deleteBucketPolicy($bucket);
 } catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to delete bucket policies, see [DeleteBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/DeleteBucketPolicy.md).

