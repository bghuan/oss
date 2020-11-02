# Delete objects

This topic describes how to delete a single object, multiple objects, or objects whose names contain a specified prefix from a versioned bucket.

## Delete operations in a versioned bucket

-   Do not specify a version ID \(temporary deletion\)

    By default, if you do not specify the version ID of the object you want to delete in the request, OSS does not delete the current version of the object and instead add a delete marker to the object as the new current version. If you perform the GetObject operation on the object, OSS identifies the current version of the object as a delete marker and returns `404 Not Found`. In addition, the `x-oss-delete-marker` header and the `x-oss-version-id` header that indicates the version ID of the created delete marker are included in the response.

-   Specify a version ID \(permanent deletion\)

    If you specify the version ID of the object you want to delete in the request, OSS permanently deletes the version whose ID is specified by the `versionId` field in the `params` parameter. To delete a version whose ID is null, add `params['versionId'] = "null"` in `params` in the request. OSS identifies the string "null" as the ID of the version to delete and deletes the version whose ID is null.


## Delete a single object

The following examples describe how to permanently or temporarily delete a single object from a versioned bucket.

-   Permanently delete an object from a versioned bucket

    The following code provides an example on how to permanently delete an object with a specified version ID from a versioned bucket:

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
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    
    try{
        // Delete the object with the specified version ID.
        $ossClient->deleteObject($bucket, $object, array(OssClient::OSS_VERSION_ID => $versionId));
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");        
    ```

-   Temporarily delete an object from a versioned bucket

    The following code provides an example on how to temporarily delete an object from a versioned bucket:

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
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    
    try{
        // Temporarily delete an object without specifying its version ID. A delete marker is added to the object.
        $ret = $ossClient->deleteObject($bucket, $object);
        print("delete marker:" .$ret['x-oss-delete-marker'] ."\n");
        print("version id:" .$ret['x-oss-version-id']);
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");    
    ```


For more information about how to delete a single object, see [DeleteObject](/intl.en-US/API Reference/Object operations/Basic operations/DeleteObject.md).

## Delete multiple objects

The following examples describe how to permanently or temporarily delete multiple objects from a versioned bucket.

-   Permanently delete multiple objects from a versioned bucket

    The following code provides an example on how to permanently delete multiple objects with specified version IDs from a versioned bucket:

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
    use OSS\Model\DeleteObjectInfo;
    
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    $accessKeyId = "<yourAccessKeyId>";
    $accessKeySecret = "<yourAccessKeySecret>";
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    $endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    $bucket= "<yourBucketName>";
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    
    try{
        // Delete the objects with the specified version IDs.
        $objects = array(); 
        $objects[] = new DeleteObjectInfo("<yourObject1Name>", "<object1VersionId>");
        $objects[] = new DeleteObjectInfo("<yourObject2Name>", "<object2VersionId>");
        
        $deletedObjectList = $ossClient->deleteObjectVersions($bucket, $objects);
    
        // Display the deleted objects.
        if (! empty($deletedObjectList)) {
              print("deletedObjectList:\n");
              foreach ($deletedObjectList as $deletedObjectInfo) {
                print($deletedObjectInfo->getKey() . ",");
                print($deletedObjectInfo->getVersionId() . ",");
                print($deletedObjectInfo->getDeleteMarker() . ",");
                print($deletedObjectInfo->getDeleteMarkerVersionId() . "\n");
              }
        }
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");
    ```

-   Temporarily delete multiple objects from a versioned bucket

    The following code provides an example on how to temporarily delete multiple objects from a versioned bucket:

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
    use OSS\Model\DeleteObjectInfo;
    
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    $accessKeyId = "<yourAccessKeyId>";
    $accessKeySecret = "<yourAccessKeySecret>";
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    $endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    $bucket= "<yourBucketName>";
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
    
    try{
        // Temporarily delete multiple object without specifying their version IDs. Delete markers are added to the objects.
            $objects = array(); 
        $objects[] = new DeleteObjectInfo("<yourObject1Name>");
        $objects[] = new DeleteObjectInfo("<yourObject2Name>");
        
        $deletedObjectList = $ossClient->deleteObjectVersions($bucket, $objects);
    
        // Display the deleted objects.
        if (! empty($deletedObjectList)) {
              print("deletedObjectList:\n");
              foreach ($deletedObjectList as $deletedObjectInfo) {
                print($deletedObjectInfo->getKey() . ",");
                print($deletedObjectInfo->getVersionId() . ",");
                print($deletedObjectInfo->getDeleteMarker() . ",");
                print($deletedObjectInfo->getDeleteMarkerVersionId() . "\n");
              }
        }
    } catch(OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");
    ```


For more information about how to delete multiple objects, see [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md).

