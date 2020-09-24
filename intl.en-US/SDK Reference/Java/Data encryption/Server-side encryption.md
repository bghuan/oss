# Server-side encryption

OSS supports server-side encryption for uploaded data. When you upload data, OSS encrypts the data and stores the encrypted data. When you download data, OSS decrypts the data and returns the original data. The returned HTTP request header declares that the data is encrypted on the server.

## Background information

OSS provides the following server-side encryption methods:

-   Server-side encryption by using CMKs managed by KMS \(SSE-KMS\)

    When you upload an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt and decrypt a large amount of data. This method is cost-effective because you do not need to send user data to the KMS server by using networks for encryption and decryption.

    **Note:** You are charged for calling API operations when you use CMKs to encrypt or decrypt data.

-   Server-side encryption by using OSS-managed keys \(SSE-OSS\)

    When you upload an object, OSS encrypts the object on the server side by using AES-256 managed by OSS. OSS server-side encryption uses AES-256 to encrypt objects by using different data keys. AES-256 uses master keys that are regularly rotated to encrypt data keys.


**Note:**

-   Only one server-side encryption method can be used for an object at a time.
-   If you configure server-side encryption for a bucket, you can still configure the encryption method for a single object when you upload or copy it. In this case, the encryption method configured for the object takes precedence. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).
-   For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## Configure server-side encryption for a bucket

The following code provides an example on how to configure the default encryption method for a bucket. After the method is configured, all objects that are uploaded to the bucket without encryption methods configured are encrypted by using the default encryption method.

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Configure server-side encryption for the bucket.
ServerSideEncryptionByDefault applyServerSideEncryptionByDefault = new ServerSideEncryptionByDefault(SSEAlgorithm.KMS);
applyServerSideEncryptionByDefault.setKMSMasterKeyID("<yourTestKmsId>");
ServerSideEncryptionConfiguration sseConfig = new ServerSideEncryptionConfiguration();
sseConfig.setApplyServerSideEncryptionByDefault(applyServerSideEncryptionByDefault);
SetBucketEncryptionRequest request = new SetBucketEncryptionRequest("<yourBucketName>", sseConfig);
ossClient.setBucketEncryption(request);

// Shut down the OSSClient instance.
ossClient.shutdown();      
```

## Query the server-side encryption configurations of a bucket

The following code provides an example on how to query the server-side encryption configurations of a bucket:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Query the server-side encryption configurations of the bucket.
ServerSideEncryptionConfiguration sseConfig = ossClient.getBucketEncryption("<yourBucketName>");
System.out.println("get Algorithm: " + sseConfig.getApplyServerSideEncryptionByDefault().getSSEAlgorithm());
System.out.println("get kmsid: " + sseConfig.getApplyServerSideEncryptionByDefault().getKMSMasterKeyID());

// Shut down the OSSClient instance.
ossClient.shutdown();
			
```

## Delete the server-side encryption configurations of a bucket

The following code provides an example on how to delete the server-side encryption configurations of a bucket:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Delete the server-side encryption configurations of the bucket.
ossClient.deleteBucketEncryption("<yourBucketName>");

// Shut down the OSSClient instance.
ossClient.shutdown();
```

