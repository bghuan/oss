# Quick start

This topic describes how to use the OSS PHP SDK to perform routine operations, such as creating buckets, uploading objects, and downloading objects.

## Create buckets

The following code provides an example on how to create a bucket:

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

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Specify the bucket name.
$bucket = "<yourBucketName>";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    $ossClient->createBucket($bucket);
} catch (OssException $e) {
    print $e->getMessage();
}
            
```

For more information about bucket naming rules, see naming conventions in [Basic concepts](/intl.en-US/Developer Guide/Terms.md). For more information about how to create a bucket, see [Manage a bucket](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md).

For more information about endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## Upload objects

The following code provides an example on how to upload an object:

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

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Specify the bucket name.
$bucket= " <yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to upload to OSS, and must include the file extension of the object. For example, set <yourObjectName> to abc/efg/123.jpg.
$object = " <yourObjectName>";
$content = "Hi, OSS.";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    $ossClient->putObject($bucket, $object, $content);
} catch (OssException $e) {
    print $e->getMessage();
}
            
```

For more information about uploading objects, see [Upload objects](/intl.en-US/SDK Reference/PHP/Upload objects/Overview .md).

## Download objects

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

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Specify the bucket name.
$bucket= "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to download from OSS, and must include the file extension of the object. For example, set <yourObjectName> to abc/efg/123.jpg.
$object = "<yourObjectName>";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    $content = $ossClient->getObject($bucket, $object);
    print("object content: " . $content);
} catch (OssException $e) {
    print $e->getMessage();
}
            
```

For more information about downloading objects, see [Download objects](/intl.en-US/SDK Reference/PHP/Download objects/Overview.md).

## List objects

The following code provides an example on how to list objects in a specified bucket. By default, up to 100 objects are listed.

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

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Specify the bucket name.
$bucket= "<yourBucketName>";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $listObjectInfo = $ossClient->listObjects($bucket);
    $objectList = $listObjectInfo->getObjectList();
    if (! empty($objectList)) {
        foreach ($objectList as $objectInfo) {
        print($objectInfo->getKey() . "\t" . $objectInfo->getSize() . "\t" . $objectInfo->getLastModified() . "\n");
        }
    }
} catch (OssException $e) {
    print $e->getMessage();
}
            
```

For more information about listing objects, see [List objects](/intl.en-US/SDK Reference/PHP/Manage objects/List objects.md) in [Manage objects](/intl.en-US/SDK Reference/PHP/Manage objects/Overview.md).

## Delete objects

The following code provides an example on how to delete an object:

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

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Specify the bucket name.
$bucket= "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to delete from OSS, and must include the file extension of the object. For example, set <yourObjectName> to abc/efg/123.jpg.
$object = "<yourObjectName>";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    $ossClient->deleteObject($bucket, $object);
} catch (OssException $e) {
    print $e->getMessage();
}
            
```

For more information about deleting objects, see [Delete objects](/intl.en-US/SDK Reference/PHP/Manage objects/Delete objects.md) in Manage objects.

