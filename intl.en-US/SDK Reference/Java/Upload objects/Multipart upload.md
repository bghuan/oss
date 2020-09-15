# Multipart upload

By using the multipart upload feature provided by OSS, you can split a large object into multiple parts and upload them separately. After all parts are uploaded, call the CompleteMultipartUpload operation to combine these parts into a single object to implement resumable upload.

## Multipart upload process

To implement multipart upload, perform the following operations:

1.  Initiate a multipart upload task.

    Call the ossClient.initiateMultipartUpload method to obtain an upload ID that is unique in OSS.

2.  Upload the parts.

    Call the ossClient.uploadPart method to upload the parts.

    **Note:**

    -   Part numbers identify the relative positions of parts in an object that share the same upload ID. If you have uploaded a part and used its part number again to upload another part, the latter part overwrites the former part.
    -   OSS includes the MD5 hash of part data in the ETag header and returns the MD5 hash to the user.
    -   OSS calculates the MD5 hash of uploaded data and compares it with the MD5 hash calculated by the SDK. If the two hashes are different, the InvalidDigest error code is returned.
3.  Complete the multipart upload task.

    After all parts are uploaded, call the ossClient.completeMultipartUpload method to combine the parts into a complete object.


For more information about multipart upload and its applicable scenarios, see [Multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md). For the complete code used to perform multipart upload, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/MultipartUploadSample.java).

## Complete sample code of multipart upload

The following code provides a complete example that describes the process of multipart upload:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
// <yourObjectName> indicates the complete path of the object you want to upload to OSS, and must include the file extension of the object. Example: abc/efg/123.jpg.
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Create an InitiateMultipartUploadRequest request.
InitiateMultipartUploadRequest request = new InitiateMultipartUploadRequest("<yourBucketName>", "<yourObjectName>");

// Optional. Specify the storage class.
// ObjectMetadata metadata = new ObjectMetadata();
// metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
// request.setObjectMetadata(metadata);

// Initiate a multipart upload task.
InitiateMultipartUploadResult upresult = ossClient.initiateMultipartUpload(request);
// Obtain an upload ID. The upload ID uniquely identifies the multipart upload task. You can use the upload ID to initiate related requests such as canceling and querying a multipart upload task.
String uploadId = upresult.getUploadId();

// partETags is a set of PartETags. A PartETag consists of an ETag and a part number.
List<PartETag> partETags =  new ArrayList<PartETag>();
// Calculate the total number of parts to upload.
final long partSize = 1 * 1024 * 1024L;   // 1MB
final File sampleFile = new File("<localFile>");
long fileLength = sampleFile.length();
int partCount = (int) (fileLength / partSize);
if (fileLength % partSize ! = 0) {
    partCount++;
 }
// Upload each part sequentially until all parts are uploaded.
for (int i = 0; i < partCount; i++) {
    long startPos = i * partSize;
    long curPartSize = (i + 1 == partCount) ? (fileLength - startPos) : partSize;
    InputStream instream = new FileInputStream(sampleFile);
    // Skip the uploaded parts.
    instream.skip(startPos);
    UploadPartRequest uploadPartRequest = new UploadPartRequest();
    uploadPartRequest.setBucketName(bucketName);
    uploadPartRequest.setKey(objectName);
    uploadPartRequest.setUploadId(uploadId);
    uploadPartRequest.setInputStream(instream);
    // Configure the size of each part. Each part except for the last part must be larger than 100 KB in size.
    uploadPartRequest.setPartSize(curPartSize);
    // Configure part numbers. Each part is configured with a part number. The number ranges from 1 to 10000. If you configure a number beyond the range, OSS returns an InvalidArgument error code.
    uploadPartRequest.setPartNumber( i + 1);
    // Parts are not necessarily uploaded in order. They may be uploaded from different OSS clients. OSS sorts the parts based on their part numbers and combines them to obtain a complete object.
    UploadPartResult uploadPartResult = ossClient.uploadPart(uploadPartRequest);
    // When a part is uploaded, OSS returns a result that contains a PartETag. The PartETag is stored in partETags.
    partETags.add(uploadPartResult.getPartETag());
}


// Create a CompleteMultipartUploadRequest request.
// When the multipart upload task is complete, you must provide all valid partETags. After OSS receives the partETags, OSS verifies the validity of all parts one by one. After all parts are validated, OSS combines these parts into a complete object.
CompleteMultipartUploadRequest completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(bucketName, objectName, uploadId, partETags);

// Optional. Set the ACL.
// completeMultipartUploadRequest.setObjectACL(CannedAccessControlList.PublicRead);

// Complete multipart upload.
CompleteMultipartUploadResult completeMultipartUploadResult = ossClient.completeMultipartUpload(completeMultipartUploadRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## Cancel a multipart upload task

You can call the ossClient.abortMultipartUpload method to cancel a multipart upload task. If a multipart upload task is canceled, the upload ID can no longer be used to upload any part. The uploaded parts are deleted.

The following code provides an example on how to cancel a multipart upload task:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Cancel the multipart upload task. The upload ID is returned by InitiateMultipartUpload.
AbortMultipartUploadRequest abortMultipartUploadRequest =
        new AbortMultipartUploadRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
ossClient.abortMultipartUpload(abortMultipartUploadRequest);

// Shut down the OSSClient instance.
ossClient.shutdown();            
```

## List uploaded parts

You can call the ossClient.listParts method to list all parts that are uploaded using the specified upload ID.

-   Simple list

    The following code provides an example for simple list:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List uploaded parts. The upload ID is returned by InitiateMultipartUpload.
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
     // Configure an upload ID.
     //listPartsRequest.setUploadId(uploadId);
     // Set the maximum number of parts to list on each page to 100. By default, 1,000 parts are listed.
     listPartsRequest.setMaxParts(100);
     // Specify the start point of the list. Only the parts whose part numbers are greater than the value of this parameter are listed.
     listPartsRequest.setPartNumberMarker(2);
    PartListing partListing = ossClient.listParts(listPartsRequest);
    
    for (PartSummary part : partListing.getParts()) {
        // Query part numbers.
        part.getPartNumber();
        // Query the part size.
        part.getSize();
        // Query the ETag.
        part.getETag();
        // Query the last modification time.
        part.getLastModified();
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

-   List all uploaded parts

    By default, listParts can list up to 1,000 parts at a time. The following code provides an example on how to list more than 1,000 parts:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List all uploaded parts.
    PartListing partListing;
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
    
    do {
        partListing = ossClient.listParts(listPartsRequest);
    
        for (PartSummary part : partListing.getParts()) {
            // Query part numbers.
            part.getPartNumber();
            // Query the part size.
            part.getSize();
            // Query the ETag.
            part.getETag();
            // Query the last modification time.
            part.getLastModified();
        }
    // Specify the start point of the list. Only the parts whose part numbers are greater than the value of this parameter are listed.
    listPartsRequest.setPartNumberMarker(partListing.getNextPartNumberMarker());
    } while (partListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

-   List all uploaded parts by page

    The following code provides an example on how to specify the maximum number of parts to list per page and list all uploaded parts by page:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List all uploaded parts by page.
    PartListing partListing;
    ListPartsRequest listPartsRequest = new ListPartsRequest("<yourBucketName>", "<yourObjectName>", "<uploadId>");
    // Set the maximum number of parts to list per page to 100.
    listPartsRequest.setMaxParts(100);
    
    do {
        partListing = ossClient.listParts(listPartsRequest);
    
        for (PartSummary part : partListing.getParts()) {
            // Query part numbers.
            part.getPartNumber();
            // Query the part size.
            part.getSize();
            // Query the ETag.
            part.getETag();
            // Query the last modification time.
            part.getLastModified();
        }
    
        listPartsRequest.setPartNumberMarker(partListing.getNextPartNumberMarker());
    
    } while (partListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```


## List multipart upload tasks

You can call the ossClient.listMultipartUploads method to list all ongoing multipart upload tasks. Ongoing multipart upload tasks are tasks that are initiated but not completed or tasks that are canceled. The following table describes the parameters of this operation.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|prefix|Specifies the prefix that returned object names must contain. Note that if you use a prefix for a query, the returned object name contains the prefix.|ListMultipartUploadsRequest.setPrefix\(String prefix\)|
|delimiter|Groups objects by name. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.|ListMultipartUploadsRequest.setDelimiter\(String delimiter\)|
|maxUploads|Specifies the maximum number of multipart upload tasks to return each time. The default value is 1000. The maximum value is 1000.|ListMultipartUploadsRequest.setMaxUploads\(Integer maxUploads\)|
|keyMarker|Specifies the name of the object after which the listing of multipart upload tasks begins. All multipart upload tasks with objects whose names start with a letter that comes after the keyMarker parameter value in alphabetical order are listed. You can use this parameter with the uploadIdMarker parameter to specify the start point to list the returned results.|ListMultipartUploadsRequest.setKeyMarker\(String keyMarker\)|
|uploadIdMarker|UploadIdMarker is used with the keyMarker parameter to specify the start point to list the returned results. If the keyMarker parameter is not configured, the uploadIdMarker parameter is invalid. If the keyMarker parameter is configured, the query result includes: -   All objects whose names start with a letter that comes after the keyMarker parameter value in alphabetical order.
-   All objects whose names start with a letter that is the same as the keyMarker parameter value in alphabetical order and of which the uploadId parameter value is greater than the uploadIdMarker parameter value.

|ListMultipartUploadsRequest.setUploadIdMarker\(String uploadIdMarker\)|

-   Simple list

    The following code provides an example on simple list:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List multipart upload tasks. By default, 1,000 tasks are listed.
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    MultipartUploadListing multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
    for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
        // Query upload IDs.
        multipartUpload.getUploadId();
        // Query object names.
        multipartUpload.getKey();
        // Query the time the multipart upload tasks are initiated.
        multipartUpload.getInitiated();
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                   
    ```

    If the value of the isTruncated field in the returned result is false, values of nextKeyMarker and nextUploadIdMarker are returned and used as the start point for the next reading. If the response does not contain all multipart upload tasks, list multipart upload tasks by page.

-   List all multipart upload tasks

    By default, listMultipartUploads can list up to 1,000 multipart upload tasks at a time. The following code provides an example on how to list more than 1,000 multipart upload tasks:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List multipart upload tasks.
    MultipartUploadListing multipartUploadListing;
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    
    do {
        multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
        for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
            // Query upload IDs.
            multipartUpload.getUploadId();
            // Query object names.
            multipartUpload.getKey();
            // Query the time the multipart upload tasks are initiated.
            multipartUpload.getInitiated();
        }
    
        listMultipartUploadsRequest.setKeyMarker(multipartUploadListing.getNextKeyMarker());
    
        listMultipartUploadsRequest.setUploadIdMarker(multipartUploadListing.getNextUploadIdMarker());
    } while (multipartUploadListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```

-   List all multipart upload tasks by page

    The following code provides an example on how to list all multipart upload tasks by page:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // List multipart upload tasks.
    MultipartUploadListing multipartUploadListing;
    ListMultipartUploadsRequest listMultipartUploadsRequest = new ListMultipartUploadsRequest(bucketName);
    // Specify the number of multipart upload tasks to list on each page.
    listMultipartUploadsRequest.setMaxUploads(50);
    
    do {
        multipartUploadListing = ossClient.listMultipartUploads(listMultipartUploadsRequest);
    
        for (MultipartUpload multipartUpload : multipartUploadListing.getMultipartUploads()) {
            // Query upload IDs.
            multipartUpload.getUploadId();
            // Query object names.
            multipartUpload.getKey();
            // Query the time the multipart upload tasks are initiated.
            multipartUpload.getInitiated();
        }
    
        listMultipartUploadsRequest.setKeyMarker(multipartUploadListing.getNextKeyMarker());
        listMultipartUploadsRequest.setUploadIdMarker(multipartUploadListing.getNextUploadIdMarker());
    
    } while (multipartUploadListing.isTruncated());
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();                    
    ```


