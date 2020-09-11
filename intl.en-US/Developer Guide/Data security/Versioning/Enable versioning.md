# Enable versioning

When versioning is enabled for a bucket, OSS generates a unique ID for each version of all objects in the bucket. The content and ACL of existing objects in the bucket remain unchanged. Versioning prevents your data from being accidentally overwritten or deleted and allows you to query previous object versions or recover an object to a specified previous version.

## Usage notes

Note the following items when you perform the following operations in versioned buckets: upload objects, list objects, download objects, delete objects, and recover objects.

-   When versioning is enabled for a bucket, a current version and zero or more previous versions are stored for each object in the bucket.
-   The version ID of objects uploaded before versioning is enabled is set to null.
-   For ease of viewing, all version IDs in the following figures are in the simple format.

For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## Upload objects

When you upload an object to a versioned bucket, OSS generates a unique version ID for the object.

**Note:** OSS generates a unique version ID for objects uploaded by PutObject, PostObject, CopyObject, and MultipartUpload.

When you use the PutObject operation to upload an object with the key of example.jpg, OSS generates a unique version ID of 111111 for the object, as shown in the following figure.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40175.jpg)

When you use the PutObject operation to upload an object with the same key of example.jpg, OSS generates a new version with a unique version ID of 222222 for the object and stores the new version as the current version of the object. Version 111111 is stored as a previous version. When you use the PutObject operation to upload an object with the same key of example.jpg again, OSS generates a new version with a unique version ID of 333333 for the object and stores the new version as the current version of the object. Versions 111111 and 222222 are stored as previous versions, as shown in the following figure.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40176.jpg)

## List objects

You can call the GetBucketVersions \(ListObjectVersions\) operation to obtain the information of all object versions and delete markers in a versioned bucket.

-   Unlike GetBucketVersions \(ListObjectVersions\), the GetBucket \(ListObject\) operation returns only the current object versions that are not delete markers in a bucket.
-   Up to 1,000 object versions can be returned for a single GetBucketVersions \(ListObjectVersions\) request. You can send multiple GetBucketVersions \(ListObjectVersions\) requests to obtain all object versions in a versioned bucket.

    For example, if a bucket contains two objects whose names are example.jpg and photo.jpg. The example.jpg object has 900 versions. The photo.jpg object has 500 versions. If you send a GetBucketVersions \(ListObjectVersions\) request, the 900 versions of the example.jpg object and 100 versions of the photo.jpg object are returned. Versions are returned based on the alphabetical order of object keys first and then in the time order when the versions are stored.


As shown in the following figure, all object versions including delete markers are returned when the GetBucketVersions \(ListObjectVersions\) operation is called. Only current object versions that are not delete markers are returned when the GetBucket\(ListObject\) operation is called. Therefore, only the current version of the photo.jpg, whose version ID is 444444, is returned.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40404.jpg)

## Download objects

You can download the current or a specified version of an object from a versioned bucket.

By default, the current version of an object is returned if a GetObject request in which no version ID is specified is sent to download the object. As shown in the following figure, the current version of the object, whose version ID is 333333, is returned.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40220.jpg)

If the current version of the object to download is a delete marker, 404 Not Found is returned for the GetObject request.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40666.jpg)

To download the specified version of an object, you must specify the version ID in the GetObject request. As shown in the following figure, a request in which a version ID is specified is sent to download the object version whose ID is 222222.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6595688951/p40221.jpg)

## Delete objects

You can specify an object version ID in the DeleteObject request or configure lifecycle rules to permanently delete an object in a versioned bucket. If you do not specify an object version ID in the DeleteObject request, OSS adds a delete marker as the current version of the object.

**Note:** By default, if you do not specify a version ID in the DeleteObject request, the current version and previous versions of the object are not deleted.

In addition, you can configure the Expiration element in lifecycle rules to delete the expired current version of objects in a versioned bucket. You can also configure the NoncurrentVersionExpiration element in lifecycle rules to permanently delete the expired previous versions of objects in a versioned bucket. The two elements are described as follows:

-   The Expiration element is specified in lifecycle rules to delete the expired current version of objects. When an object is deleted based on a lifecycle rule with Expiration configured, OSS stores the current version of the object as a previous version and adds a delete marker as the new current version of the object.
-   The NoncurrentVersionExpiration element is specified in lifecycle rules to delete expired previous versions of objects. A previous version deleted based on a lifecycle with NoncurrentVersionExpiration configured is permanently deleted and cannot be recovered.

For more information about how to use versioning with lifecycle rules, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

The following examples describe how OSS deletes an object when the version ID is specified and not specified in the DeleteObject request.

-   If the version ID is not specified in the DeleteObject request, OSS add a delete marker as the current version of the object to delete. The delete marker has a unique version ID but does not have data and ACL. As shown in the following figure, a delete marker with the version ID of 444444 is added as the current version.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7595688951/p40237.jpg)

-   If the version ID is specified in the DeleteObject request, the specified version is permanently deleted. As shown in the following figure, the version whose ID is 333333 is permanently deleted.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7595688951/p40245.jpg)


## Recover objects

When versioning is enabled for a bucket, all versions of objects in the bucket are preserved. You can recover a specified previous version of an object as the current version.

You can recover a previous version of an object as the current version in the following two methods:

-   Use CopyObject to copy the previous version to the same bucket

    The copied previous version becomes the current version of the object and all versions of the object are preserved.

    In the following figure, a previous version with the version ID of 222222 is copied to the same bucket. OSS adds the copied version as a new version whose ID is 444444. The new version becomes the current version of the object. Therefore, the object has its previous version whose ID is 222222 and its copy whose ID is 444444 as the current version.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7595688951/p40309.jpg)

-   Permanently delete the current version of the object

    In the following figure, the current version whose ID is 222222 is permanently deleted by a DeleteObject request with the version ID specified, and the latest previous version whose ID is 111111 becomes the current version of the object.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7595688951/p40310.jpg)


**Note:**

-   We recommend that you use CopyObject to recover a previous version as the current version.
-   You can configure lifecycle rules to delete the previous versions of objects. For more information, see [Best practices for versioning](/intl.en-US/Best Practices/Best practices for versioning.md).

