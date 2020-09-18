# Convert the storage class of an object

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), Archive and Cold Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md) and [Convert storage classes](/intl.en-US/Developer Guide/Storage classes/Convert storage classes.md) in the OSS Developer Guide.

The following code provides an example of how to convert the storage class of an object.

-   The following code provides an example of how to convert the storage class of an object from Standard or IA to Archive:

    ```
    // This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // This example requires a bucket containing an object of the Standard or IA storage class.
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Create CopyObjectRequest.
    CopyObjectRequest request = new CopyObjectRequest(bucketName, objectName, bucketName, objectName) ;
    
    // Create ObjectMetadata.
    ObjectMetadata objectMetadata = new ObjectMetadata();
    
    // Encapsulate the header. Set the storage class to Archive.
    objectMetadata.setHeader("x-oss-storage-class", StorageClass.Archive);
    request.setNewObjectMetadata(objectMetadata);
    
    // Modify the storage class of the object.
    CopyObjectResult result = ossClient.copyObject(request);
    
    // Close your OSSClient.
    ossClient.shutdown();
    ```

-   The following code provides an example of how to convert the storage class of an object from Archive to IA:

    ```
    // This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // This example requires a bucket containing an object of the Archive storage class.
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Obtain the object metadata.
    ObjectMetadata objectMetadata = ossClient.getObjectMetadata(bucketName, objectName);
    
    // Check whether the storage class of the source object is Archive. If the storage class of the source object is Archive, you must restore the object before you can modify the storage class.
    StorageClass storageClass = objectMetadata.getObjectStorageClass();
    System.out.println("storage type:" + storageClass);
    if (storageClass == StorageClass.Archive) {
        // Restore the object.
        ossClient.restoreObject(bucketName, objectName);
    
        // Wait until the object is restored.
        do {
            Thread.sleep(1000);
            objectMetadata = ossClient.getObjectMetadata(bucketName, objectName);  
        } while (! objectMetadata.isRestoreCompleted());
    }
    
     // Create CopyObjectRequest.
    CopyObjectRequest request = new CopyObjectRequest(bucketName, objectName, bucketName, objectName) ;
    
    // Create ObjectMetadata.
    objectMetadata = new ObjectMetadata();
    
    // Encapsulate the header. Set the storage class to IA.
    objectMetadata.setHeader("x-oss-storage-class", StorageClass.IA);
    request.setNewObjectMetadata(objectMetadata);
    
    // Modify the storage class of the object.
    CopyObjectResult result = ossClient.copyObject(request);
    
    // Close your OSSClient.
    ossClient.shutdown();
    ```


For more information about the storage classes and storage fees, see the [t4320.md\#section\_jmi\_2z4\_hka](/intl.en-US/Pricing/Billing items and methods/Overview.md) section in Billing items.

