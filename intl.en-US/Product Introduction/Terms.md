# Terms

This topic describes several basic terms used in OSS.

## bucket

A container for OSS objects. Each object in OSS is contained in a bucket. You can configure various attributes of a bucket, including the region, ACL, and storage class. You can create buckets of different storage classes to store data based on your requirement.

-   OSS does not have the hierarchical structure of directories and subfolders as in a file system. All objects are directly related to their corresponding buckets.
-   You can create multiple buckets.
-   A bucket name must be globally unique within OSS. Bucket names cannot be changed after the buckets are created.
-   A bucket can contain an unlimited number of objects.

The naming conventions for buckets:

-   The name can contain only lowercase letters, digits, and hyphens \(-\).
-   The name must start and end with a lowercase letter or digit.
-   The name must be 3 to 63 bytes in length.

## object

The fundamental entities stored in OSS. Objects are also known as files. OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. An object is composed of object metadata, user data, and a key. A key is used to uniquely identify an object in a bucket. Object metadata is a group of key-value pairs that define the properties of an object, such as the last modification time and the object size. You can also assign user metadata to the object.

The lifecycle of an object starts when the object is uploaded, and ends when the object is deleted. During the lifecycle, content can be appended only to objects created by using append upload. If you want to modify an object, you must upload a new object that has the same name as the existing object to replace the existing object.

The naming conventions for objects:

-   The name can contain only UTF-8 characters.
-   The name must be 1 to 1,023 bytes in length.
-   The name cannot start with a forward slash \(/\) or a backslash \(\\\).

    **Note:** Object names are case-sensitive. Unless otherwise stated, objects and files mentioned in OSS documents are called objects.


## ObjectKey

ObjectKey, Key, and ObjectName are the same in different SDKs. They all indicate the object names that you must specify when you perform related operations on an object. For example, when you upload an object to a bucket, ObjectKey indicates the full path that includes the extension of the object. For example, you can set ObjectKey to abc/efg/123.jpg.

## region

The physical location of an OSS data center. When you create a bucket, you can select a region based on the cost and request source. In most cases, users can access OSS data centers from closer regions faster than centers from farther regions. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

A region is specified when a bucket is created. After a bucket is created, its region cannot be changed. All objects in this bucket are stored in the corresponding data center. Regions are configured for buckets instead of objects.

## endpoint

The domain name used to access OSS. OSS uses HTTP RESTful APIs to provide services. Different regions are accessed by using different endpoints. A region has different endpoints for access over the internal network and for access over the Internet. For example, the public endpoint to access OSS data in the China \(Hangzhou\) region is oss-cn-hangzhou.aliyuncs.com, and the internal endpoint is oss-cn-hangzhou-internal.aliyuncs.com. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## AccessKey pair

The access credential that is used to identify the requester. An AccessKey pair consists of an AccessKey ID and an AccessKey secret. OSS uses an AccessKey pair that includes an AccessKey ID and an AccessKey secret to implement symmetric encryption and verify the identity of a requester. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt the signature string and for OSS to verify the signature string. The AccessKey secret must be kept confidential. OSS supports AccessKey pairs obtained by using the following methods:

-   The bucket owner applies for an AccessKey pair.
-   The bucket owner uses RAM to assign the AccessKey pair to a third party.
-   The bucket owner uses STS to assign the AccessKey pair to a third party.

For more information about AccessKey pairs, see [Create an AccessKey pair]().

## Strong consistency

A feature requires that object operations in OSS be atomic, which indicates that operations can only either succeed or fail without intermediate states. To ensure that users can access only complete data, OSS does not return corrupted or partial data.

Object operations in OSS are highly consistent. For example, when a user receives an upload \(PUT\) success response, the uploaded object can be read immediately, and copies of the object are written to multiple devices for redundancy. Therefore, the situations where data is not obtained when you perform the read-after-write operation do not exist. The same is true for delete operations. After you delete an object, the object and its copies no longer exist.

## Data redundancy mechanism

OSS uses the data redundancy mechanism that is based on erasure coding and multiple replicas to store copies of each object in multiple devices across different facilities within the same region. This way, data reliability and availability are ensured even if hardware failures occur.

-   Operations on objects in OSS are highly consistent. For example, when you receive an upload or copy success response, the uploaded object can be read immediately, and the copies of the object are written to multiple devices for redundancy.
-   To ensure complete data transmission, OSS checks for errors when packets are transmitted between the client and the server. To do so, it calculates the checksum of the network traffic packets.
-   The data redundancy mechanism of OSS can prevent data loss when two storage devices are damaged at the same time.
    -   After data is stored in OSS, OSS regularly checks whether copies of the data are lost. Then, OSS recovers lost copies to ensure the reliability and availability of data.
    -   OSS periodically verifies the integrity of data to detect data corruption caused by errors such as hardware failures. If data is partially corrupted or lost, OSS reconstructs and repairs the corrupted data by using the other copies.

## Comparison between OSS and file systems

|Item|OSS|File system|
|----|---|-----------|
|data model|OSS is a distributed object storage service that uses a key-value pair format.|A file system uses a typical tree index structure.|
|Data retrieval|Objects are retrieved based on unique object names \(keys\). For example, object name test1/test.jpg does not necessarily indicate that the object is stored in a directory named test1. In OSS, test1/test.jpg is only a string. test1/test.jpg is not essentially different from a.jpg. Therefore, similar amounts of resources are consumed regardless of which object you access.

|To access a file named test1/test.jpg, you must first access the test1 directory and then search for the test.jpg file in this directory.|
|Benefits|OSS supports sporadic bursts of access from multiple users.|The file system supports modifications on files, such as modifying the content at a specified offset location or truncating the end of a file. It also supports folder operations such as renaming, deleting, and moving folders.|
|Disadvantages|Objects stored in OSS cannot be modified. A specific operation must be called to append an object, and the generated object is different from objects uploaded by using other methods. To modify even a single byte, you must upload the entire object again. You can simulate similar functions of a file system in OSS, but such operations are costly. For example, if you want to rename the test1 directory as test2, OSS must copy all objects whose names start with test1/ to generate objects whose names start with test2. This operation consumes a large amount of resources. Therefore, we recommend that you do not perform such operations in OSS.

|The performance of the file system is subject to the performance of a single device. Creating more files and directories in the file system consumes greater resources and leads to longer processing time.|

We recommend that you do not map operations on OSS objects to file systems because it is inefficient. If you attach OSS as a file system, we recommend that you only add new files, delete files, and read files. You can make full use of OSS advantages, such as the capability to process and store large amounts of unstructured data such as images, videos, and documents.

The following table lists the differences of some terms between OSS and file systems.

|OSS|File system|
|:--|:----------|
|Object|file|
|Bucket|home directory|
|Region|None|
|Endpoint|None|
|AccessKey|None|
|None|multilevel directory|
|GetService|obtain the list of home directories|
|GetBucket|obtain the list of files|
|PutObject|add a file|
|AppendObject|append data to an existing file|
|GetObject|read a file|
|DeleteObject|delete a file|
|None|modify file content|
|CopyObject \(same destination and source buckets\)|modify file attributes|
|CopyObject|copy a file|
|None|rename a file|

