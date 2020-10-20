# Server-side encryption

OSS supports server-side encryption for uploaded data. When you upload data to OSS, OSS encrypts the uploaded data and then persistently stores the encrypted data. When you download the encrypted data, OSS automatically decrypts the data, returns the original data to you, and declares in the header of the response that the data had been encrypted on the server.

## Encryption methods

OSS provides the following server-side encryption methods:

-   Server-side encryption by using KMS-managed keys \(SSE-KMS\)

    When you upload an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt the object. This method is cost-effective because you do not need to send user data to the KMS server over networks for encryption and decryption.

    You are charged for calling API operations when you use CMKs to encrypt or decrypt data. For more information about the fees, see [KMS pricing](/intl.en-US/Pricing/Billing.md).

-   Server-side encryption by using OSS-managed keys \(SSE-OSS\)

    When you upload an object, OSS encrypts the object on the server side by using AES-256 keys managed by OSS. OSS server-side encryption uses AES-256 to encrypt objects by using different data keys. AES-256 uses master keys that are regularly rotated to encrypt data keys.


**Note:**

-   An object can be encrypted by only one server-side encryption method at a time.
-   If you configure server-side encryption for a bucket, you can still configure the encryption method for a single object when you upload or copy it. In this case, the encryption method configured for the object takes precedence. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Configure server-side encryption for a bucket

The following code provides an example on how to configure the default encryption method for a bucket. After the method is configured, all objects that are uploaded to the bucket without encryption methods configured are encrypted by using the default encryption method.

The following code provides an example on how to configure the default encryption method for a bucket:

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
use OSS\Model\ServerSideEncryptionConfig;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Set the default server-side encryption method of the bucket to SSE-OSS.
    $config = new ServerSideEncryptionConfig("AES256");
    $ossClient->putBucketEncryption($bucket, $config);

    // Set the default server-side encryption method of the bucket to KMS without specifying a CMK ID.
    $config = new ServerSideEncryptionConfig("KMS");
    $ossClient->putBucketEncryption($bucket, $config);

    // Set the default server-side encryption method of the bucket to KMS and specify a CMK ID.
    $config = new ServerSideEncryptionConfig("KMS", "your kms id");
    $ossClient->putBucketEncryption($bucket, $config);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n"); 
```

For more information about how to configure server-side encryption, see [PutBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/PutBucketEncryption.md).

## Query the server-side encryption configurations of a bucket

The following code provides an example on how to query the server-side encryption configurations of a bucket:

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
use OSS\Model\ServerSideEncryptionConfig;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

try {
    // Query the server-side encryption configurations of the bucket.
    $config = $ossClient->getBucketEncryption($bucket);

    // Display the server-side encryption configurations of the bucket.
    print($config->getSSEAlgorithm());
    print($config->getKMSMasterKeyID());
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");   
```

For more information about how to query the server-side encryption configurations of a bucket, see [GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md).

## Delete the server-side encryption configurations of a bucket

The following code provides an example on how to delete the server-side encryption configurations of a bucket:

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
    // Delete the server-side encryption configurations of the bucket.
    $ossClient->deleteBucketEncryption($bucket);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");   
```

For more information about how to delete the server-side encryption configurations of a bucket, see [DeleteBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/DeleteBucketEncryption.md).

