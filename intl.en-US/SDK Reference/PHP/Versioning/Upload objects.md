# Upload objects

This topic describes how to upload objects to a versioned bucket.

## Simple upload

If you initiate a PutObject request to upload an object to a versioned bucket, OSS generates a globally unique version ID for the uploaded object and includes the version ID in the response as the `x-oss-version-id` header.

If you initiate a PutObject request to upload an object to a bucket for which versioning is suspended, OSS generates a version ID null for the uploaded object. In this case, if you upload a new object with the same name as the existing object to the bucket, the existing object is overwritten.

The following code provides an example on how to upload an object to a versioned bucket by using simple upload:

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
$content = "hello world";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

try {
    // Upload the object to the versioned bucket.
    $ret = $ossClient->putObject($bucket, $object, $content);

    // Display the version ID of the uploaded object.
    print("versionId:" .$ret[OssClient::OSS_HEADER_VERSION_ID]);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

For more information about simple upload, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Append upload

When you upload an object to a versioned bucket by using append upload, take note of the following items:

-   You can perform the AppendObject operation only on an object whose current version is an append object.
-   The AppendObject operation cannot be performed on an object whose current version is not an append object, such as a normal object or a delete marker.
-   When you perform the AppendObject operation on an object whose current version is an append object, OSS does not generate a previous version for the append object.
-   When you perform the PutObject or DeleteObject operation on an object whose current version is an append object, OSS stores the append object as a previous version and prevents the object from being further appended.

The following code provides an example on how to upload an object to a versioned bucket by using append upload:

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
// Specifies that the object content obtained after the first, second, and third append upload is Hello OSS, Hi OSS, and OSS OK.
$content_array = array('Hello OSS', 'Hi OSS', 'OSS OK');
try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    // Perform the first append upload. // If the object is appended for the first time, the append position is 0. The return value is the position for the next append. The position to start the next append is the total length of appended bytes and the append object.
    $position = $ossClient->appendObject($bucket, $object, $content_array[0], 0);    
    $position = $ossClient->appendObject($bucket, $object, $content_array[1], $position);   
    $position = $ossClient->appendObject($bucket, $object, $content_array[2], $position);
} catch(OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
```

For more information about append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Multipart upload

If you call the CompleteMultipartUpload operation to complete the MultipartUpload task for an object in a versioned bucket, OSS generates a unique version ID for the uploaded object and returns the version ID in the response as the `x-oss-version-id` header.

The following code provides an example on how to upload an object to a versioned bucket by using multipart upload:

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
use OSS\Core\OssUtil;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";
// Specify the complete path of the local file to upload.
$uploadFile = "<yourLocalFile>";

/**
 * Step 1: Initiate a multipart upload task and obtain the upload ID.
 */
try{
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    // Obtain the upload ID. // The upload ID uniquely identifies the multipart upload task. You can use the upload ID to cancel or query the multipart upload task.
    $uploadId = $ossClient->initiateMultipartUpload($bucket, $object);
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
$uploadFileSize = filesize($uploadFile);
$pieces = $ossClient->generateMultiuploadParts($uploadFileSize, $partSize);
$responseUploadPart = array();
$uploadPosition = 0;
$isCheckMd5 = true;
foreach ($pieces as $i => $piece) {
    $fromPos = $uploadPosition + (integer)$piece[$ossClient::OSS_SEEK_TO];
    $toPos = (integer)$piece[$ossClient::OSS_LENGTH] + $fromPos - 1;
    $upOptions = array(
        // Upload the object.
        $ossClient::OSS_FILE_UPLOAD => $uploadFile,
        // Configure part numbers.
        $ossClient::OSS_PART_NUM => ($i + 1),
        // Specify the position from which the multipart upload task starts.
        $ossClient::OSS_SEEK_TO => $fromPos,
        // Specify the object size.
        $ossClient::OSS_LENGTH => $toPos - $fromPos + 1,
        // Specify whether to enable MD5 verification. A value of true indicates MD5 verification is enabled. A value of false indicates MD5 verification is disabled.
        $ossClient::OSS_CHECK_MD5 => $isCheckMd5,
    );
    // Enable MD5 verification.
    if ($isCheckMd5) {
        $contentMd5 = OssUtil::getMd5SumForFile($uploadFile, $fromPos, $toPos);
        $upOptions[$ossClient::OSS_CONTENT_MD5] = $contentMd5;
    }
    try {
        // Upload the parts.
        $responseUploadPart[] = $ossClient->uploadPart($bucket, $object, $uploadId, $upOptions);
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": initiateMultipartUpload, uploadPart - part#{$i} FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    printf(__FUNCTION__ . ": initiateMultipartUpload, uploadPart - part#{$i} OK\n");
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
 * Step 3: Complete the multipart upload task.
 */
try {
    // All valid $uploadParts are required for the CompleteMultipartUpload operation. OSS verifies the validity of each part after it receives the submitted $uploadParts. After all parts are verified, OSS combines these parts into a complete object.
    $ret = $ossClient->completeMultipartUpload($bucket, $object, $uploadId, $uploadParts);
    // Display the version ID of the uploaded object.
    print("versionId:" .$ret[OssClient::OSS_HEADER_VERSION_ID]);
}  catch(OssException $e) {
    printf(__FUNCTION__ . ": completeMultipartUpload FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
printf(__FUNCTION__ . ": completeMultipartUpload OK\n");   
```

For more information about multipart upload, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md), [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md), and [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

