# Versioning

The versioning state of a bucket applies to all objects in the bucket. Versioning allows you to restore objects in a bucket to any previous version and protects your data from being accidentally overwritten or deleted.

A bucket can be in one of the following versioning states: unversioned \(default\), enabled, or suspended. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) in OSS Developer Guide.

**Note:** If versioning is enabled for a bucket, the versioning state of the bucket cannot be set back to unversioned. However, you can set the versioning state of the bucket to suspended.

## Set the versioning state of a bucket

The following code provides an example on how to set the versioning state of a bucket:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Set the versioning state of the bucket to enabled or suspended.
    $ossClient->putBucketVersioning($bucket, "Enabled");
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to set the versioning state of a bucket, see [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md).

## Query the versioning state of a bucket

The following code provides an example on how to query the versioning state of a bucket:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Query the versioning state of the bucket.
    $status = $ossClient->getBucketVersioning($bucket);
    print($status);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to query the versioning state of a bucket, see [GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md).

## Lists the versions of all objects in a bucket.

The following code provides an example on how to list the versions of all objects and delete markers in a bucket:

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

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {

    $maxKey = 100;
    $nextKeyMarker = '';
    $nextVersionIdMarker = '';

    while (true) {
        $options = array(
            'delimiter' => '',
            'key-marker' => $nextKeyMarker,
            'max-keys' => $maxKey,
            'version-id-marker' => $nextVersionIdMarker,
        );
        $result = $ossClient->listObjectVersions($bucket, $options);

        $nextKeyMarker = $result->getNextKeyMarker();
        $nextVersionIdMarker = $result->getNextVersionIdMarker();
       
        $objectList = $result->getObjectVersionList();
        $deleteMarkerList = $result->getDeleteMarkerList();
        
        // Display the versions of all objects in the bucket.
        if (! empty($objectList)) {
            print("objectList:\n");
            foreach ($objectList as $objectInfo) {
                print($objectInfo->getKey() . ",");
                print($objectInfo->getVersionId() . ",");
                print($objectInfo->getLastModified() . ",");
                print($objectInfo->getETag() . ",");
                print($objectInfo->getSize() . ",");
                print($objectInfo->getIsLatest() . "\n");
            }
        }
        
        // Display the version information about all delete markers in the bucket.
        if (! empty($deleteMarkerList)) {
            print("deleteMarkerList: \n");
            foreach ($deleteMarkerList as $deleteMarkerInfo) {
                print($deleteMarkerInfo->getKey() . ",");
                print($deleteMarkerInfo->getVersionId() . ",");
                print($deleteMarkerInfo->getLastModified() . ",");
                print($deleteMarkerInfo->getIsLatest() . "\n");
            }
        }

                if ($result->getIsTruncated() ! == "true") {
                break;
        }
    }
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about how to list the versions of all objects and delete markers in a bucket, see [GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md).

