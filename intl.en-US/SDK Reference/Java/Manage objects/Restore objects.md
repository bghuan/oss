# Restore objects

This topic describes how to restore objects of the Archive and Cold Archive storage classes.

**Note:** You must restore an archived or cold archived object before you can read it. Do not call RestoreObject to perform restore operations on objects of which the storage class is not Archive and Cold Archive. For more information about the Archive and Cold Archive storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about related status codes, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) in API Reference.

## Restore archived objects

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

For the complete code to restore archived objects, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/test/java/com/aliyun/oss/integrationtests/ArchiveTest.java).

The following code provides an example on how to restore an archived object:

```
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements. 
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com. 
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

ObjectMetadata objectMetadata = ossClient.getObjectMetadata(bucketName, objectName);

// Verify whether the object is an archived object.
StorageClass storageClass = objectMetadata.getObjectStorageClass();
if (storageClass == StorageClass.Archive) {
    // Restore the object.
    ossClient.restoreObject(bucketName, objectName);

    // Wait until the object is restored.
    do {
        Thread.sleep(1000);
        objectMetadata = ossClient.getObjectMetadata(bucketName, objectName);
    } while (! objectMetadata.isRestoreCompleted());
}

// Obtain the restored object. 
OSSObject ossObject = ossClient.getObject(bucketName, objectName);
ossObject.getObjectContent().close();

// Shut down the OSSClient instance.
ossClient.shutdown();
            
```

## Restore cold archived objects

The status of a cold archived object throughout the restoration process is described as follows:

1.  By default, the cold archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore a cold archived object:

```
// This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements. 
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourColdArchiveObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Refer to the following code if you set the storage class of the object to upload to Cold Archive. 
// Create a PutObjectRequest object. 
// PutObjectRequest putObjectRequest = new PutObjectRequest(bucketName, objectName, new ByteArrayInputStream("<yourContent>".getBytes()));
// Set the storage class of the object to Cold Archive in the object metadata.
// ObjectMetadata metadata = new ObjectMetadata();
// metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.ColdArchive.toString());
// putObjectRequest.setMetadata(metadata);
// Configure the storage class of the object to upload. 
// ossClient.putObject(putObjectRequest);

// Configure the restore mode for the cold archived object. 
// RestoreTier.RESTORE_TIER_EXPEDITED: The object is restored within an hour. 
// RestoreTier.RESTORE_TIER_STANDARD: The object is restored within two to five hours.
// RestoreTier.RESTORE_TIER_BULK: The object is restored within five to twelve hours.
RestoreJobParameters jobParameters = new RestoreJobParameters(restoreTier);

// Configure parameters. For example, set the restore mode for the object to Standard, and set the duration for which the object can remain in the restored state to two days.
// The first parameter indicates the validity period of the restored state. The default value is one day. This parameter applies to archived and cold archived objects. 
// The jobParameters indicates the restore mode of the object and applies only to cold archived objects. 
RestoreConfiguration configuration = new RestoreConfiguration(2, jobParameters);

// Initiate a restore request. 
ossClient.restoreObject(bucketName, objectName, configuration);

// Shut down the OSSClient instance. 
ossClient.shutdown();
```

