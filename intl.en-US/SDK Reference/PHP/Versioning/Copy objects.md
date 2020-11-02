# Copy objects

This topic describes how to copy objects in a versioned bucket. You can use CopyObject to copy an object that is up to 1 GB in size and use UploadPartCopy to copy an object larger than 1 GB.

## Copy a small object

You can use CopyObject to copy an object that is up to 1 GB in size from a source bucket to a destination bucket in the same region.

-   By default, the x-oss-copy-source header in a CopyObject request specifies the current version of the object to copy. If the current version of the object to copy is a delete marker, OSS returns 404 Not Found to indicate that the object does not exist. You can add a version ID in the x-oss-copy-source header to copy a specified version of the object. Delete markers cannot be copied.
-   You can copy a previous version of an object to the same bucket. The copied previous version becomes the current version of the object. This way, you can restore an object to a specified previous version.
-   If versioning is enabled for the destination bucket, OSS generates a unique version ID for the new object. The version ID is returned as the x-oss-version-id header in the response. If versioning is disabled or suspended for the destination bucket, OSS generates a version whose version ID is null for the destination object and overwrites the original version whose version ID is null.
-   Append objects in a bucket for which versioning is enabled or suspended cannot be copied.

The following code provides an example on how to copy a small object:

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
$versionId = "<yourObjectVersionId>";
$to_bucket = $bucket;
$to_object = $object . '.copy';

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try{
    // Copy the object with the specified version ID.
    $ret = $ossClient->copyObject($bucket, $object, $to_bucket, $to_object, array(OssClient::OSS_VERSION_ID => $versionId));
    print("versionId:" .$ret['x-oss-version-id']);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");    
            
```

For more information about how to copy small objects, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

## Copy a large object

To copy an object larger than 1 GB, you must split the object into parts and copy them sequentially by using UploadPartCopy.

By default, the UploadPartCopy operation uploads a part by copying data from the current version of an existing object. To copy data from a specific version of an object, include the versionId parameter in the `x-oss-copy-source` header in the request as a subcondition. For example, you can set x-oss-copy-source to `x-oss-copy-source : /SourceBucketName/SourceObjectName?versionId=111111`.

**Note:** The value of SourceObjectName must be URL-encoded. The version ID of the copied object version is returned as the x-oss-copy-source-version-id header value in the response.

If versionId is not specified in the request and the current version of the source object is a delete marker, OSS returns 404 Not Found. If versionId is specified in the request and the specified version of the source object is a delete marker, OSS returns 400 Bad Request.

The following code provides an example on how to use multipart copy to copy a large object:

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
$to_bucket = $bucket;
$to_object = $object . '.copy';

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

/**
 * Step 1: Initiate a multipart upload task and obtain the upload ID.
 */
try {
    // Obtain the upload ID. The upload ID is the unique identifier for a multipart upload task. You can perform related operations such as cancel or query the multipart upload task based on the upload ID.
    $uploadId = $ossClient->initiateMultipartUpload($to_bucket, $to_object);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": initiateMultipartUpload FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": initiateMultipartUpload OK" . "\n");

/*
 * Step 2: Upload parts.
 */
$partSize = 10 * 1024 * 1024;
try {
    $meta = $ossClient->getSimplifiedObjectMeta($bucket, $object);
    $uploadSize = $meta[strtolower('Content-Length')];
} catch(OssException $e) {
    printf(__FUNCTION__ . ": getSimplifiedObjectMeta FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
$pieces = $ossClient->generateMultiuploadParts($uploadSize, $partSize);
$responseUploadPart = array();
$uploadPosition = 0;
foreach ($pieces as $i => $piece) {
    $fromPos = $uploadPosition + (integer)$piece[$ossClient::OSS_SEEK_TO];
    $toPos = (integer)$piece[$ossClient::OSS_LENGTH] + $fromPos - 1;
    $upOptions = array(
        // Specify the position of the source object from which the multipart copy task starts .
        'start' => $fromPos,
        // Specify the position of the source object at which the multipart copy task ends. 
        'end' => $toPos,
        // Specify the version ID of the source object you want to copy.
        OssClient::OSS_VERSION_ID => $versionId
    );
    try {
        // Upload the parts.
        $responseUploadPart[] = $ossClient->uploadPartCopy($bucket, $object, $to_bucket, $to_object, ($i + 1), $uploadId, $upOptions);
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": initiateMultipartUpload, uploadPartCopy - part#{$i} FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    printf(__FUNCTION__ . ": initiateMultipartUpload, uploadPartCopy - part#{$i} OK\n");
}
// $uploadParts is an array that consists of the ETag and the PartNumber of each part.
$uploadParts = array();
foreach ($responseUploadPart as $i => $eTag) {
    $uploadParts[] = array(
        'PartNumber' => ($i + 1),
        'ETag' => $eTag,
    );
}

/**
 /* Step 3: Complete multipart upload task.
 */
try {
    // All valid $uploadParts are required for the CompleteMultipartUpload operation. OSS verifies the validity of each part after it receives the submitted $uploadParts. After all parts are verified, OSS combines these parts into a complete object.
    $ret = $ossClient->completeMultipartUpload($to_bucket, $to_object, $uploadId, $uploadParts);
    // Display the version ID of the copied object.
    print("versionId:" .$ret[OssClient::OSS_HEADER_VERSION_ID]);
}  catch(OssException $e) {
    printf(__FUNCTION__ . ": completeMultipartUpload FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
printf(__FUNCTION__ . ": completeMultipartUpload OK\n");     
```

For more information about multipart copy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).

