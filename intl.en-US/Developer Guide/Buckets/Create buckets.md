# Create buckets

Before you upload objects to OSS, you must call the PutBucket operation to create a bucket to store objects. You can configure various attributes for a bucket, including the region, access control list \(ACL\), and other metadata.

**Note:** For more information about the PutBucket operation, see [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/mb.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)|SDK demos in various languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Buckets/Create buckets.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Manage buckets.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Create buckets.md)|

## Limits

-   You can create up to 100 buckets in a region.
-   The name of each bucket must be globally unique in OSS. To create a bucket, you must select a unique bucket name.
-   Bucket names must comply with the bucket naming conventions.
-   After a bucket is created, you cannot modify its name or region.
-   You must complete real-name registration on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page before you create a bucket in a region within mainland China.

## ACL

You can set the ACL when you create a bucket, or modify the ACL for a created bucket. If you do not set ACL, the default value private applies. For more information, see [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md).

## References

-   [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md)
-   [Simple download](/intl.en-US/Developer Guide/Objects/Download files/Simple download.md)
-   [Delete a bucket](/intl.en-US/Developer Guide/Buckets/Delete a bucket.md)
-   [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md)

