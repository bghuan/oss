# Bucket tagging

You can use tags to identify buckets used for different purposes and manage these buckets by category.

## Usage notes

A tag is a key-value pair that is used to identify a bucket. When you configure tags for a bucket, take note of the following items.

-   Only the owner of a bucket and authorized RAM users can configure tags for the bucket. If other users attempt to configure tags for the bucket, 403 Forbidden is returned with the error code AccessDenied.
-   You can configure up to 20 tags for a bucket. The key and value of a tag must be encoded in UTF-8.
-   The key of a tag can be up to 64 bytes in length and cannot start with `http://`, `https://`, or `Aliyun`. The key of a tag cannot be empty.
-   The value of a tag can be up to 128 bytes in length and can be empty.
-   When you use PutBucketTags to configure a tag for a bucket, the new tag overwrites the exiting tag.

For more information about bucket tagging, see [Configure bucket tagging](/intl.en-US/Developer Guide/Buckets/Configure bucket tagging.md).

## Configure bucket tags

The following code provides an example on how to configure tags for a bucket:

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
use OSS\Model\TaggingConfig;
use OSS\Model\Tag;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Configure tags for the bucket.
    $config = new TaggingConfig();
    $config->addTag(new Tag("key1", "value1"));
    $config->addTag(new Tag("key2", "value2"));
    $ossClient->putBucketTags($bucket, $config);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");  
```

For more information about how to configure bucket tags, see [PutBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/PutBucketTags.md).

## Query bucket tags

The following code provides an example on how to query the tags configured for a bucket:

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
use OSS\Model\TaggingConfig;
use OSS\Model\Tag;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Query the tags configured for the bucket.
    $config = $ossClient->getBucketTags($bucket);
    print_r($config->getTags());
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n"); 
```

For more information about how to query the tags configured for a bucket, see [GetBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/GetBucketTags.md).

## Delete bucket tags

The following code provides an example on how to delete the tags configured for a bucket:

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
use OSS\Model\Tag;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Delete specific tags configured for the bucket.
    $tags = array();
    $tags[] = new Tag("key1", "value1");
    $tags[] = new Tag("key2", "value2");
    $ossClient->deleteBucketTags($bucket, $tags);

    // Delete all tags configured for the bucket.
    $ossClient->deleteBucketTags($bucket);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n"); 
```

For more information about how to delete the tags configured for a bucket, see [DeleteBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/DeleteBucketTags.md).

