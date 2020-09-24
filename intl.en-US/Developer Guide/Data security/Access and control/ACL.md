# ACL

OSS provides access control lists \(ACLs\) for you to control access to OSS resources. You can configure ACL for buckets and objects to control access. You can set the bucket or object ACL when you create a bucket or upload an object. You can also modify the ACL of a created bucket or an uploaded object at any time.

**Note:** For more information about ACL-based OSS API operations, see the following topics:

-   Set the ACL for a bucket: [PutBucketACL](/intl.en-US/API Reference/Bucket operations/ACL/PutBucketACL.md)
-   Query the ACL of a bucket: [GetBucketACL](/intl.en-US/API Reference/Bucket operations/ACL/GetBucketAcl.md)
-   Set the ACL for an object: [PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md)
-   Query the ACL of an object: [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md)

## Bucket ACL

-   Overview

    Bucket ACLs are used to control access to buckets. The following table describes the ACLs for objects.

    |ACL|Description|
    |:--|:----------|
    |public-read-write|Anyone, including anonymous users can read, write, and delete objects in the bucket. Fees incurred by such operations are paid by the owner of the bucket. Configure this permission only when necessary.|
    |public-read|Only the bucket owner can write objects in the bucket. Other users, including anonymous users can perform only read operations on objects in the bucket.|
    |private|Only the bucket owner or authorized users can read and write objects in the bucket. Other users, including anonymous users cannot access the objects in the bucket without authorization.|

-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Modify bucket ACLs.md)|A user-friendly and intuitive web application|
    |[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-acl.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage bucket ACLs.md)|SDK demos for various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Manage bucket ACLs.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage bucket ACLs.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)|
    |[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)|
    |[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Create buckets.md)|


## Object ACL

-   Overview

    Object ACLs are used to control access to objects. The following ACL types are available: private, public-read, public-read-write, and default. You can set the ACL for an object by including the `x-oss-object-acl` header in the PUT request for the PutObjectACL operation. Only the owner of a bucket can perform the PutObjectACL operation on the objects in the bucket.

    The following table describes the ACL types for objects.

    |ACL|Description|
    |:--|:----------|
    |public-read-write|Any users, including anonymous users can read and write the object.|
    |public-read|The object is a public read resource. Only the owner or authorized users of the bucket can write the object. Other users, including anonymous users can only read the object.|
    |private|The object is a private resource. Only the owner or authorized users of the bucket can read and write the object.|
    |default|The ACL of an object is the same as that of the bucket in which the object is stored.|

    **Note:**

    -   If you do not set the object ACL, the object ACL is default. The ACL of the object is the same as that of the bucket in which the object is stored.
    -   If an ACL is set for an object, the object ACL takes precedence over the ACL of the bucket where the object is stored. Example: If the ACL for an object is set to public-read, all authenticated and anonymous users can read the object regardless of the bucket ACL.
-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure ACL for objects.md)|A user-friendly and intuitive web application|
    |[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-acl.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Manage ACL for an object.md)|SDK demos for various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Manage ACL for an object.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/Manage ACL for an object.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Manage ACL for an object.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Manage objects/Manage ACL for an object.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/Manage ACL for an object.md)|


