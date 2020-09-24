# Set the ACL for a bucket

You can set the access control list \(ACL\) when you create a bucket, or modify the ACL for a created bucket. Only bucket owners can set or modify the ACL for buckets.

The following table describes the three types of ACLs for buckets.

|ACL|Description|Description|
|:--|:----------|:----------|
|public-read-write|Public read/write|Anyone \(including anonymous users\) can read and write the objects in the bucket. **Warning:** All Internet users can access the objects in the bucket and write data to the bucket. This may cause unexpected access to the data in your bucket, and cause an increase in your fees. If a user uploads prohibited data or information, it may affect your legitimate interests and rights. Therefore, we recommend that you do not set your bucket ACL to Public Read/Write except in special cases. |
|public-read|Public read|Only the bucket owner can perform write operations on the objects in the bucket. Other users \(including anonymous users\) can perform only read operations on the objects in the bucket. **Warning:** All Internet users can access the objects in the bucket. This configuration may cause unwanted access to the data in your bucket, and cause an increase in your fees. Therefore, we recommend that you exercise caution when you set your bucket ACL to Public Read. |
|private|Private|Only the bucket owner can perform read and write operations on the objects in the bucket. Other users have no access to the objects in the bucket.|

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Modify bucket ACLs.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-acl.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage bucket ACLs.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Manage bucket ACLs.md)|
|[PHP SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage bucket ACLs.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage bucket ACLs.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Manage bucket ACLs.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Manage bucket ACLs.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Manage bucket ACLs.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Manage bucket ACLs.md)|

## References

-   [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md)
-   [Overview]()

