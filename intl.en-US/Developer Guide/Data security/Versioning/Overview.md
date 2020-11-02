# Overview

OSS allows you to configure versioning to protect data at the bucket level. When versioning is enabled for a bucket, bucket data that is overwritten or deleted is saved as a previous version. You can use versioning to recover objects in a bucket to a previous version and protect your data from being accidentally overwritten or deleted.

## Limits

When you use versioning, note the following items:

-   Permissions

    Only the bucket owner or RAM users that have the PutBucketVersioning permission can configure versioning for a bucket.

-   Feature conflicts
    -   A bucket cannot have both versioning and retention policy or mirroring-based back-to-origin configured at the same time.
    -   If versioning is enabled for a bucket, the `x-oss-forbid-overwrite` request header that is specified when an object is uploaded to the bucket does not take effect. For more information, see [Request headers](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).
-   Supported regions

    Versioning is supported in all regions except Japan \(Tokyo\).


## Implementation modes

You can use one of the following methods to configure versioning for buckets.

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Versioning.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/bucket-versioning.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Versioning/Versioning.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Versioning/Versioning.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Versioning/Versioning.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Versioning/Versioning.md)|
|[.NET](/intl.en-US/SDK Reference/. NET/Versioning/Versioning.md)|
|[Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Versioning.md)|

## Versioning status

A bucket can be in one of the following versioning states: unversioned, enabled, or suspended.

-   By default, the versioning status of a bucket is unversioned. After versioning is enabled for a bucket, the versioning state of the bucket cannot be returned to unversioned. However, you can suspend versioning for a versioning-enabled bucket.
-   When an object is uploaded to a bucket for which versioning is enabled, OSS generates a random string as the globally unique version ID of the object. For more information about how to perform operations on objects in a versioning-enabled bucket, see [Enable versioning](/intl.en-US/Developer Guide/Data security/Versioning/Enable versioning.md).
-   When an object is uploaded to a bucket for which versioning is suspended, OSS generates a string null as the version ID of the object. For more information about how to perform operations on objects in a bucket for which versioning is suspended, see [Suspend versioning](/intl.en-US/Developer Guide/Data security/Versioning/Suspend versioning.md).

**Note:** In a versioning-enabled bucket, all versions of an object are stored, which consumes storage space and incurs storage fees. To reduce storage costs, we recommend that you configure lifecycle rules based on scenarios to delete unnecessary previous versions or convert the storage class of current or previous object versions to IA or Archive. For more information, see [Best practices for versioning](/intl.en-US/Best Practices/Best practices for versioning.md).

## Scenarios

To ensure data security, we recommend that you configure versioning in the following scenarios:

-   Recover deleted data

    OSS does not provide the recycle bin feature. You can configure versioning to recover deleted data.

-   Recover overwritten data

    Online collaborative documents and documents stored in online storage are frequently mondified. In online office scenarios, a large number of temporary versions are generated when objects are edited. You can configure versioning to recover data to a specified version of a point in time.


## Data protection

The following table describes how OSS processes deleted and overwritten data in buckets in different versioning states to help you understand the data protection mechanism of versioning.

|Versioning status|Data overwritten|Object deletion|
|-----------------|----------------|---------------|
|Unversioned|The existing object is overwritten and cannot be recovered. Only the current object version can be accessed.|The object is deleted and cannot be accessed.|
|Enabled|A new version with a unique ID is added for the object. The existing object is stored as a previous version.|A delete marker with a globally unique version ID is added to the object as the current version. The existing object is stored as a previous version.|
|Suspended|A new version that contains the null version ID is added for the object.If a previous version or delete marker whose version ID is null already exists, the previous version or delete marker is overwritten. Other objects or delete markers whose version ID is not null are unaffected.

|A delete marker that contains the null version ID is added for the object.If a previous version or delete marker whose version ID is null already exists, the previous version or delete marker is overwritten by the new delete marker. Other objects or delete markers whose version IDs are not null are unaffected. |

The following figures show how OSS processes data when an object with the same name as that of an existing object is uploaded to or an object is deleted from a bucket for which versioning is enabled or suspended. For ease of illustration, all version IDs in the figures are in the simple format.

-   Overwrite an object in a versioning-enabled bucket

    When you upload an object repeatedly to a versioning-enabled bucket, the object is overwritten for multiple times. A version with a unique version ID is generated for the object each time when the object is overwritten.

    ![1](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4944169951/p143835.png)

-   Delete an object from a versioning-enabled bucket

    When you delete an object from a versioning-enabled bucket, the previous versions of the object are not deleted, and a delete marker is added to the object as the current version to indicate that the object is deleted. If you upload an object that has the same name after the delete marker is added, a new version with a unique version ID is added as the current version.

    ![2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4944169951/p143867.png)

-   Overwrite an object in a bucket for which versioning is suspended

    When you upload an object to a bucket for which versioning is suspended, a new version whose version ID is null is added and the previous versions of the object are retained. If you upload the object that has the same name again, a new version whose version ID is null overwrites the current version whose version ID is null.

    ![3](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5944169951/p143879.png)

-   Delete an object from a bucket for which versioning is suspended

    When you delete an object from a bucket for which versioning is suspended, the previous versions of the object are not deleted and a delete marker is added to the object as the current version to indicate that the object is deleted.

    ![4](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5944169951/p143882.png)


As described in the preceding examples, deleted and overwritten data is stored as previous versions. Versioning allows you to recover objects in a bucket to any previous point in time, and protects your data from being accidentally overwritten or deleted.

## FAQ

Why does the response speed decrease significantly when the GetBucket \(ListObjects\) operation is called to list current object versions in a versioning-enabled bucket?

Cause: One or more objects in your bucket have a large number of previous versions or expired delete markers.

Troubleshooting:

-   Call the GetBucketVersions \(ListObjectVersions\) operation to check whether the objects in your bucket have a large number of previous versions. For more information, see [GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md).
-   Use the bucket inventory function to view the information about the objects in your bucket and check whether the objects have previous versions or expired delete markers. For more information, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).

Solution: Configure lifecycle rules for your bucket and specify the NonCurrentVersionExpiration and ExpiredObjectDeleteMarker operations in the rules to delete expired previous object versions and delete markers. For more information, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

