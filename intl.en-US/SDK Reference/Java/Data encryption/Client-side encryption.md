# Client-side encryption

Client-side encryption is used to encrypt objects on the local client before they are uploaded to OSS.

## Disclaimer

-   When you use client-side encryption, you must ensure the integrity and validity of the Customer Master Key \(CMK\). If the CMK is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.
-   When you copy or migrate encrypted data, you must ensure the integrity and validity of the object metadata related to client-side encryption. If the object metadata related to client-side encryption is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.

## Background information

In client-side encryption, a random data key is generated for each object to perform symmetric encryption on the object. The client uses a CMK to encrypt the random data key. The encrypted data key is uploaded as a part of the object metadata and stored in the OSS server. When an encrypted object is downloaded, the client uses the CMK to decrypt the random data key and then uses the data key to decrypt the object. The CMK is used only on the client and is not transmitted over the network or stored in the server, which ensures data security.

**Note:**

-   Client-side encryption supports multipart upload for objects larger than 5 GB. When you use multipart upload to upload an object, you must specify the total size of the object and the size of each part. The size of each part except for the last part must be the same and be a multiple of 16 bytes.
-   After you upload objects encrypted on the client, object metadata related to client-side encryption is protected and you cannot call [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) to modify object metadata related to client-side encryption.

## Encryption methods

You can use CMKs managed in one of the following ways:

-   Use CMKs managed by Key Management Service \(KMS\)

    When you use a CMK stored in KMS for client-side encryption, you must send the CMK ID to OSS SDK.

-   Use CMKs managed by yourself \(RSA\)

    When you use a CMK managed by yourself for client-side encryption, you must send the public key and the private key of your CMK to OSS SDK as parameters.


You can use the preceding methods to prevent data leaks and protect your data on the client. Even if your data is leaked, it cannot be decrypted by others.

## Object metadata related to client-side encryption

|Parameter|Description|Required|
|:--------|:----------|:-------|
|x-oss-meta-client-side-encryption-key|The encrypted data key. The encrypted data key is a string encrypted by using a CMK and encoded in Base64.|Yes|
|x-oss-meta-client-side-encryption-start|The initial value randomly generated for data encryption. The initial value is a string encrypted by using a CMK and encoded in Base64.|Yes|
|x-oss-meta-client-side-encryption-cek-alg|The encryption algorithm used to encrypt data.|Yes|
|x-oss-meta-client-side-encryption-wrap-alg|The encryption algorithm used to encrypt data keys.|Yes|
|x-oss-meta-client-side-encryption-matdesc|The description of the CMK. The description is in the JSON format. **Warning:** We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and its description. A CMK without a specified description cannot be replaced.

|No|
|x-oss-meta-client-side-encryption-unencrypted-content-length|The length of data before encryption. This parameter is not generated if content-length is not specified.|No|
|x-oss-meta-client-side-encryption-unencrypted-content-md5|The MD5 hash of the data before encryption. This parameter is not generated if Content-MD5 is not specified.|No|
|x-oss-meta-client-side-encryption-data-size|The total size of the data to encrypt for multipart upload when init\_multipart is called.|Yes \(for multipart upload\)|
|x-oss-meta-client-side-encryption-part-size|The size of each part to encrypt for multipart upload when init\_multipart is called. **Note:** The size of each part must be a multiple of 16 bytes.

|Yes \(for multipart upload\)|

## Create a client for client-side encryption

**Note:** When you use client-side encryption, ensure that you use OSS SDK for Java V3.9.1 or later and add the Bouncy Castle Crypto package to your project. For example, you can add the following dependency to the pom.xml file:

```
<dependency>
    <groupId>org.bouncycastle</groupId>
    <artifactId>bcprov-jdk15on</artifactId>
    <version>1.62</version>
</dependency>
```

During the operation, if the `java.security.InvalidKeyException: Illegal key size or default parameters` exception occurs, you must add the Java Cryptography Extension \(JCE\) jurisdiction policy file of Oracle and deploy this file in the JRE environment.

Download the corresponding file based on the OSS JDK version you use and decompress the file and save them in the jre/lib/security directory.

-   [Java Cryptography Extension \(JCE\) Unlimited Strength Jurisdiction Policy Files 6 Download](https://www.oracle.com/java/technologies/jce-6-download.html)
-   [Java Cryptography Extension \(JCE\) Unlimited Strength Jurisdiction Policy Files 7 Download](https://www.oracle.com/java/technologies/javase-jce7-downloads.html)
-   [Java Cryptography Extension \(JCE\) Unlimited Strength Jurisdiction Policy Files 8 Download](https://www.oracle.com/java/technologies/javase-jce8-downloads.html)

The following sections provide complete sample code on how to create a client for RSA-based encryption and for KMS-based encryption.

-   Create a client for RSA-based encryption

    Before you create a client for RSA-based encryption, you must create an asymmetric key pair. OSS SDK for Java provides methods for converting a PKCS \#1-encoded or PKCS \#8-encoded private key string in the PEM format to an RSA private key, and methods for converting an X.509-encoded public key string in the PEM format to an RSA public key.

    You can use the following methods to convert keys that meet the preceding requirements:

    `RSAPrivateKey SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(String privateKeyStr);`

    `RSAPrivateKey SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS8(String privateKeyStr);`

    `RSAPublicKey SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(String publicKeyStr);`

    The following code provides an example on how to create a client for RSA-based encryption:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // You can run the following commands to separately generate the PEM files for the private key and public key. Then, set the values of PRIVATE_PKCS1_PEM and PUBLIC_X509_PEM to the strings included in the PEM files for the private key and public key.
    // openssl genrsa -out private_key.pem 2048
    // openssl rsa -in private_key.pem -out rsa_public_key.pem -pubout
    
    // Enter your RSA private key string.
    final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
    // Enter your RSA public key string.
    final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";
    
    // Create an RSA key pair.
    RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
    RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
    KeyPair keyPair = new KeyPair(publicKey, privateKey);
    
    // Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
    // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
    // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
    // We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
    Map<String, String> matDesc = new HashMap<String, String>();
    matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");
    
    // Create RSA encryption materials.
    SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
    // To download and decrypt objects encrypted by using other CMKs, add these CMKs and their descriptions to the encryption materials.
    // encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);
    
    // Create a client for RSA-based encryption.
    OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                            build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);
    ```

-   Create a client for KMS-based encryption

    The following code provides an example on how to create a client for KMS-based encryption:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    // Specify the CMK ID.
    String cmk = "<yourCmkId>";
    // Specify the region to which the CMK belongs.
    String region = "<yourKmsRegion>";
    
    
    // Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description and one region for a CMK.
    // If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
    // If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
    // We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
    Map<String, String> matDesc = new HashMap<String, String>();
    matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");
    
    // Create KMS encryption materials.
    KmsEncryptionMaterials encryptionMaterials = new KmsEncryptionMaterials(region, cmk, matDesc); 
    
    // To download and decrypt objects encrypted by using other CMKs, add the region names and descriptions of the CMKs to the KMS encryption materials.
    // encryptionMaterials.addKmsDescMaterial(<otherKmsRegion>, <otherKmsMatDesc>);
    // If you use different accounts to log on to KMS and OSS and want to download and decrypt objects encrypted by using other CMKs, add the region names, credentials, and descriptions of the CMKs to the KMS encryption materials.
    // encryptionMaterials.addKmsDescMaterial(<otherKmsRegion>, <otherKmsCredentialsProvider>, <otherKmsMatDesc>);
    
    // Create a client for KMS-based encryption.
    OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);
    ```


The following sections provide complete sample code for using customer-managed CMKs \(RSA-based\) to encrypt objects for simple upload and download, multipart upload, and range download.

**Note:** KMS-based client-side encryption differs from RSA-based client-side encryption only in the creation of OSSEncryptionClient.

## Perform RSA-based client-side encryption in simple upload and download

The following code provides an example on how to use a CMK managed by yourself to encrypt objects in simple upload or decrypt objects in simple download:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String content = "Hello OSS!" ;

// Enter your RSA private key string.
final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
// Enter your RSA public key string.
final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";

// Create an RSA key pair.
RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
KeyPair keyPair = new KeyPair(publicKey, privateKey);

// Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
// If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
// If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
// We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
Map<String, String> matDesc = new HashMap<String, String>();
matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");

// Create RSA encryption materials.
SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
// To download and decrypt objects encrypted by using other CMKs, add these CMKs and their descriptions to the encryption materials.
// encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);

// Create a client for client-side encryption.
OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);
// Encrypt the object you want to upload.
ossEncryptionClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// Download the object. The object is automatically decrypted.
OSSObject ossObject = ossEncryptionClient.getObject(bucketName, objectName);
BufferedReader reader = new BufferedReader(new InputStreamReader(ossObject.getObjectContent()));
StringBuffer buffer = new StringBuffer();
String line;
while ((line = reader.readLine()) ! = null) {
    buffer.append(line);
}
reader.close();

// Check whether the decrypted content is the same as the uploaded object in plaintext.
System.out.println("Put plain text: " + content);
System.out.println("Get and decrypted text: " + buffer.toString());
```

## Perform RSA-based client-side encryption in multipart upload

The following code provides an example on how to use a customer-managed CMK \(RSA-based\) to encrypt objects in multipart upload:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String localFile = "<yourLocalFilePath>";

// Enter your RSA private key string.
final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
// Enter your RSA public key string.
final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";

// Create an RSA key pair.
RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
KeyPair keyPair = new KeyPair(publicKey, privateKey);


// Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
// If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
// If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
// We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
Map<String, String> matDesc = new HashMap<String, String>();
matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");

// Create RSA encryption materials.
SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
// To download and decrypt objects encrypted by using other CMKs, add these CMKs and their descriptions to the encryption materials.
// encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);

// Create a client for client-side encryption.
OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);

// Create a MultipartUploadCryptoContext object and specify the sizes of the object and each part. The part size must be a multiple of 16 bytes.
File file = new File(localFile);
long fileLength = file.length();
final long partSize = 100 * 1024L;   // 100K
MultipartUploadCryptoContext context = new MultipartUploadCryptoContext();
context.setPartSize(partSize);
context.setDataSize(fileLength);

// Initiate a multipart upload task.
InitiateMultipartUploadRequest initiateMultipartUploadRequest = new InitiateMultipartUploadRequest(bucketName, objectName);
// Upload the created MultipartUploadCryptoContext object.
InitiateMultipartUploadResult upresult = ossEncryptionClient.initiateMultipartUpload(initiateMultipartUploadRequest, context);
String uploadId = upresult.getUploadId();

// Create a set of PartETags. A PartETag consists of an ETag and a part number.
List<PartETag> partETags =  new ArrayList<PartETag>();
int partCount = (int) (fileLength / partSize);
if (fileLength % partSize ! = 0) {
    partCount++;
}

// Upload each part sequentially until all parts are uploaded.
for (int i = 0; i < partCount; i++) {
    long startPos = i * partSize;
    long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
    InputStream instream = new FileInputStream(file);
    instream.skip(startPos);
    UploadPartRequest uploadPartRequest = new UploadPartRequest();
    uploadPartRequest.setBucketName(bucketName);
    uploadPartRequest.setKey(objectName);
    uploadPartRequest.setUploadId(uploadId);
    uploadPartRequest.setInputStream(instream);
    uploadPartRequest.setPartSize(curPartSize);
    uploadPartRequest.setPartNumber( i + 1);
    // Upload the created MultipartUploadCryptoContext object.
    UploadPartResult uploadPartResult = ossEncryptionClient.uploadPart(uploadPartRequest, context);
    partETags.add(uploadPartResult.getPartETag());
}

// Complete the multipart upload task.
CompleteMultipartUploadRequest completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);
ossEncryptionClient.completeMultipartUpload(completeMultipartUploadRequest);

// Shut down OSSEncryptionClient.
ossEncryptionClient.shutdown();
```

## Perform RSA-based client-side encryption in resumable upload

The following code provides an example on how to use a customer-managed CMK \(RSA-based\) to encrypt objects in resumable upload:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String localFile = "<yourLocalFilePath>";

// Enter your RSA private key string, which can be generated by using OpenSSL.
final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
// Enter your RSA public key string, which can be generated by using OpenSSL.
final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";

// Create an RSA key pair.
RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
KeyPair keyPair = new KeyPair(publicKey, privateKey);

// Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
// If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
// If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
// We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
Map<String, String> matDesc = new HashMap<String, String>();
matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");

// Create RSA encryption materials.
SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
// To download and decrypt objects encrypted by other CMKs, add these CMKs and their descriptions to the encryption materials.
// encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);

// Create a client for client-side encryption.
OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);

// Create an UploadFileRequest object.   
UploadFileRequest uploadFileRequest = new UploadFileRequest(bucketName, key);

// Set the path of the object you want to upload.
uploadFileRequest.setUploadFile(localFile));
// Specify the size of each part to upload. Valid values: 100 KB to 5 GB. The default size of each part is calculated based on the following formula: Default size of each part = Object size/10000.
uploadFileRequest.setPartSize(100 * 1024);
// Specify whether to enable resumable upload. By default, resumable upload is disabled.
uploadFileRequest.setEnableCheckpoint(true);
// Configure the checkpoint file. If you do not specify the checkpoint file, the default name localfile + ".ucp" is used. localfile + ".ucp" is in the same directory as localfile.
This file stores the information about upload progress. If you fail to upload a part, the part upload continues based on the recorded progress. After the object is uploaded, the checkpoint file is deleted.
uploadFileRequest.setCheckpointFile("test-upload.ucp");

// Start resumable upload.
ossEncryptionClient.uploadFile(uploadFileRequest);

// Shut down OSSEncryptionClient.
ossEncryptionClient.shutdown();
```

## Perform RSA-based client-side encryption in resumable download

The following code provides an example on how to use a customer-managed CMK \(RSA-based\) to decrypt objects in resumable download:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Enter your RSA private key string, which can be generated by using OpenSSL.
final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
// Enter your RSA public key string, which can be generated by using OpenSSL.
final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";

// Create an RSA key pair.
RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
KeyPair keyPair = new KeyPair(publicKey, privateKey);

// Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
// If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
// If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
// We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
Map<String, String> matDesc = new HashMap<String, String>();
matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");

// Create RSA encryption materials.
SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
// To download and decrypt objects encrypted by using other CMKs, add these CMKs and their descriptions to the encryption materials.
// encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);

// Create a client for client-side encryption.
OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);

// Send a request to start resumable download in which 10 download tasks run concurrently.
DownloadFileRequest downloadFileRequest = new DownloadFileRequest(bucketName, objectName);
downloadFileRequest.setDownloadFile("<yourDownloadFile>");
downloadFileRequest.setPartSize(1 * 1024 * 1024);
downloadFileRequest.setTaskNum(10);
downloadFileRequest.setEnableCheckpoint(true);
downloadFileRequest.setCheckpointFile("<yourCheckpointFile>");

// Start resumable download.
DownloadFileResult downloadRes = ossEncryptionClient.downloadFile(downloadFileRequest);

// Shut down OSSEncryptionClient.
ossEncryptionClient.shutdown();
```

## Perform RSA-based client-side encryption in range download

The following code provides an example on how to use a customer-managed CMK \(RSA-based\) to decrypt objects in range download:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
String content = "test-range-get-content-82042795hlnf12s8yhfs976y2nfoshhnsdfsf235bvsmnhtskbcfd!" ;

// Enter your RSA private key string.
final String PRIVATE_PKCS1_PEM = "<yourPrivateKeyWithPKCS1PemString>";
// Enter your RSA public key string.
final String PUBLIC_X509_PEM = "<yourPublicKeyWithX509PemString>";

// Create an RSA key pair.
RSAPrivateKey privateKey = SimpleRSAEncryptionMaterials.getPrivateKeyFromPemPKCS1(PRIVATE_PKCS1_PEM);
RSAPublicKey publicKey = SimpleRSAEncryptionMaterials.getPublicKeyFromPemX509(PUBLIC_X509_PEM);
KeyPair keyPair = new KeyPair(publicKey, privateKey);

// Specify the description of the CMK. The description cannot be modified after it is specified. You can specify only one description for a CMK.
// If all objects share a CMK, you can leave the description of the CMK empty. In this case, the CMK cannot be replaced.
// If the description of the CMK is empty, the client cannot determine which CMK to use for decryption.
// We recommend that you configure a description for each CMK and store the mapping relationship between the CMK and the description in the client. The server does not store the relationship.
Map<String, String> matDesc = new HashMap<String, String>();
matDesc.put("<yourDescriptionKey>", "<yourDescriptionValue>");

// Create RSA encryption materials.
SimpleRSAEncryptionMaterials encryptionMaterials = new SimpleRSAEncryptionMaterials(keyPair, matDesc);
// To download and decrypt objects encrypted by using other CMKs, add these CMKs and their descriptions to the encryption materials.
// encryptionMaterials.addKeyPairDescMaterial(<otherKeyPair>, <otherKeyPairMatDesc>);

// Create a client for client-side encryption.
OSSEncryptionClient ossEncryptionClient = new OSSEncryptionClientBuilder().
                        build(endpoint, accessKeyId, accessKeySecret, encryptionMaterials);
// Encrypt the object you want to upload.
ossEncryptionClient.putObject(bucketName, objectName, new ByteArrayInputStream(content.getBytes()));

// Download the object by range.
int start = 17;
int end = 35;
GetObjectRequest getObjectRequest = new GetObjectRequest(bucketName, objectName);
getObjectRequest.setRange(start, end);
ossObject = ossEncryptionClient.getObject(getObjectRequest);
reader = new BufferedReader(new InputStreamReader(ossObject.getObjectContent()));
buffer = new StringBuffer();
while ((line = reader.readLine()) ! = null) {
    buffer.append(line);
}
reader.close();

// View the download result.
System.out.println("Range-Get plain text:" + content.substring(start, end + 1));
System.out.println("Range-Get decrypted text: " + buffer.toString());

// Shut down OSSEncryptionClient.
ossEncryptionClient.shutdown();
```

