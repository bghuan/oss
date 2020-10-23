# Delete objects

This topic describes how to delete a single object, multiple objects, or objects whose names contain a specified prefix from a versioned bucket.

## Delete operations in versioned bucket

When you delete an object from a versioned bucket, you must determine whether to specify a version ID in the request.

-   Do not specify a version ID \(temporary deletion\)

    By default, if you do not specify a version ID in a DeleteObject request, OSS does not delete the current version of the object but add a delete marker to the object as the new current version. If you perform the GetObject operation on the object, OSS identifies that the current version of the object is a delete marker, and then returns `404 Not Found`. In addition, the `x-oss-delete-marker` header and the `x-oss-version-id` header that indicates the version ID of the created delete marker are included in the response to the DeleteObject request.

    The value of the `x-oss-delete-marker` header in the response is true, which indicates that the `x-oss-version-id` is the version ID of a delete marker.

-   Specify a version ID \(permanent deletion\)

    If you specify a version ID in a DeleteObject request, OSS permanently deletes the version whose ID is specified by the `versionId` field in the `params` parameter. To delete a version whose ID is null, add `params['versionId'] = "null"` in `params` in the request. OSS identifies the string "null" as the ID of the version to delete, and deletes the version whose ID is null.


## Delete a single object

The following examples describe how to permanently or temporarily delete a single object from a versioned bucket.

-   Permanently delete an object from a versioned bucket

    The following code provides an example on how to permanently delete an object from a versioned bucket by specifying the version ID of the object in the request:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    // Specify the complete name of the object to delete, including the object extension. Example: example/test.jpg.
    String objectName = "<yourObjectName>";
    // You can specify the version ID of an object or a delete marker.
    String versionid = "<yourObjectVersionidOrDelMarkerVersionid>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Delete the object with the specified version ID.
    ossClient.deleteVersion(bucketName, objectName , versionid);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Temporarily delete an object from a versioned bucket

    The following code provides an example on how to temporarily delete an object from a versioned bucket by sending a request in which no version ID is specified:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Temporarily delete the object. A delete marker is added to the object.
    ossClient.deleteObject(bucketName, objectName);
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


For more information about how to delete a single object, see [DeleteObject](/intl.en-US/API Reference/Object operations/Basic operations/DeleteObject.md).

## Delete multiple objects

The following examples describe how to permanently or temporarily delete multiple objects from a versioned bucket.

-   Permanently delete multiple objects from a versioned bucket

    The following code provides an example on how to permanently delete multiple objects or delete markers from a versioned bucket by specifying the version IDs of the objects or delete markers in the request:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Permanently delete the objects or delete markers that have specified version IDs.
    List<KeyVersion> keyVersionsList = new ArrayList<KeyVersion>();
    keyVersionsList.add(new KeyVersion("<yourObject3Name>","<yourObject3NameVersionid>"));
    keyVersionsList.add(new KeyVersion("<yourObject4Name>","<yourObject4DelMarkerVersionid>"));
    DeleteVersionsRequest delVersionsRequest = new DeleteVersionsRequest(bucketName);
    delVersionsRequest.setKeys(keyVersionsList);
    // Initiate a deleteVersions request.
    DeleteVersionsResult delVersionsResult = ossClient.deleteVersions(delVersionsRequest);
    
    // Display the deleted objects or delete markers.
    for (DeletedVersion delVer : delVersionsResult.getDeletedVersions()) {
      String keyName = URLDecoder.decode(delVer.getKey(), "UTF-8");
      String keyVersionId = delVer.getVersionId();
      String keyDelMarkerId = delVer.getDeleteMarkerVersionId();
      System.out.println("delete key: " + keyName);
      System.out.println("delete key versionid: " + keyVersionId);
      if(keyDelMarkerId ! = null && keyDelMarkerId.length() ! = 0){
        System.out.println("delete key del_marker versionid: " + keyDelMarkerId);
      }
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Temporarily delete multiple objects from a versioned bucket

    The following code provides an example on how to delete multiple objects by sending a request in which no version ID is specified:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String versionid = "<yourObjectVersionid>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Delete multiple objects.
    List<String> KeysList = new ArrayList<String>();
    KeysList.add("<yourObject1Name>");
    KeysList.add("<yourObject2Name>");
    DeleteObjectsRequest request = new DeleteObjectsRequest(bucketName);
    request.setKeys(KeysList);
    // Initiate a deleteObjects request.
    DeleteObjectsResult delObjResult = ossClient.deleteObjects(request);
    
    // Display the deleted objects.
    for (String o : delObjResult.getDeletedObjects()) {
        String keyName = URLDecoder.decode(o, "UTF-8");
        System.out.println("delete key name: " + keyName);
    }
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


For more information about how to delete multiple objects, see [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md).

## Delete objects whose names contain a specified prefix

The following code provides an example on how to delete objects whose names contain a specified prefix:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Specify the prefix.
final String prefix = "<yourkeyPrefix>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// List the versions of all objects whose names contain the specified prefix and delete the versions.
String nextKeyMarker = null;
String nextVersionMarker = null;
VersionListing versionListing = null;
do {
    ListVersionsRequest listVersionsRequest = new ListVersionsRequest()
            .withBucketName(bucketName)
            .withKeyMarker(nextKeyMarker)
            .withVersionIdMarker(nextVersionMarker)
            .withPrefix(prefix);

    versionListing = ossClient.listVersions(listVersionsRequest);
    if (versionListing.getVersionSummaries().size() > 0) {
        List<KeyVersion> keyVersionsList = new ArrayList<KeyVersion>();
        for (OSSVersionSummary ossVersion : versionListing.getVersionSummaries()) {
            System.out.println("key name: " + ossVersion.getKey());
            System.out.println("versionid: " + ossVersion.getVersionId());
            System.out.println("Is delete marker: " + ossVersion.isDeleteMarker());
            keyVersionsList.add(new KeyVersion(ossVersion.getKey(), ossVersion.getVersionId()));
        }
        DeleteVersionsRequest delVersionsRequest = new DeleteVersionsRequest(bucketName).withKeys(keyVersionsList);
        ossClient.deleteVersions(delVersionsRequest);
    }

    nextKeyMarker = versionListing.getNextKeyMarker();
    nextVersionMarker = versionListing.getNextVersionIdMarker();
} while (versionListing.isTruncated());

// Shut down the OSSClient instance.
ossClient.shutdown();
```

