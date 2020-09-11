# ACL-based access control

OSS provides access control lists \(ACLs\) for you to control access permissions. ACLs are access policies that grant bucket and object access permissions to users. You can set an ACL when creating a bucket or uploading an object, and modify the ACL for a created bucket or an uploaded object at any time.

**Note:** For more information about ACL-based OSS API operations, see the following topics:

-   Set the ACL for a bucket: [PutBucketACL](/intl.en-US/API Reference/Bucket operations/ACL/PutBucketACL.md)
-   Obtain the ACL of a bucket: [GetBucketACL](/intl.en-US/API Reference/Bucket operations/ACL/GetBucketAcl.md)
-   Set the ACL for an object: [PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md)
-   Obtain the ACL of an object: [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md)

## Bucket ACL

-   Overview

    Bucket ACLs are used to control access to buckets. The following ACL types are available: public-read-write, public-read, and private. These ACL types are described in the following table.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|The public-read-write permission.|Anyone, including anonymous users, can perform read, write, and delete operations on objects in the bucket. Fees incurred by such operations are paid by the owner of the bucket. Configure this permission only when necessary.|
    |public-read|The public-read permission.|Only the bucket owner or authorized users can perform write and delete operations on objects in the bucket. Other users, including anonymous users, can only read from the objects in the bucket.|
    |private|The private permission.|Only the bucket owner or authorized users can perform read, write, and delete operations on objects in the bucket. Other users cannot access the objects in the bucket without authorization.|

-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Modify bucket ACLs.md)|A user-friendly and intuitive Web application|
    |[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|Easy-to-operate graphical tool|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-acl.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)|SDK demos in various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)|
    |[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)|
    |[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Create buckets.md)|


## Object ACL

-   Overview

    Object ACLs are used to control access to objects. The following ACL types are available: private, public-read, public-read-write, and default. You can set the ACL for an object by including the `x-oss-object-acl` field in the request header of a PUT request for the PutObjectACL operation. Only the owner of a bucket can perform the PutObjectACL operation on the objects in the bucket.

    The following table describes the ACL types for objects.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|The public-read-write permission.|All users can read data from and write data to the object.|
    |public-read|The public-read permission.|The object owner can read data from and write data to the object. Other users can only read data from the object.|
    |private|The private permission.|The object owner can read data from and write data to the object. Other users have no access to the object without authorization.|
    |default|The default permission.|The object inherits the ACL of the bucket where it is stored.|

    **Note:**

    -   If no ACL is set for an object, the object uses the default ACL, indicating that the object has the same ACL as the bucket where the object is stored.
    -   If an ACL is set for an object, the object ACL takes precedence over the ACL of the bucket where the object is stored. Example: If the ACL for an object is set to public-read, all authenticated and anonymous users can read data from the object regardless of the bucket ACL.
-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure ACL for objects.md)|A user-friendly and intuitive Web application|
    |[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|Easy-to-operate graphical tool|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-acl.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md)|SDK demos in various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/Manage ACL for an object.md)|


