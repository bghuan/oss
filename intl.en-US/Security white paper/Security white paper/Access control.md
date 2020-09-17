# Access control

OSS provides access control list \(ACL\), RAM and bucket policies, and hotlink protection based on Referer whitelists to control and manage access to your OSS resources.

## Read and write permissions

OSS provides access control lists \(ACLs\) for you to control access permissions. ACLs are policies that grant users access permissions on buckets and objects. You can set the bucket or object ACL when creating a bucket or uploading an object. You can also modify the ACL of a created bucket or an uploaded object at any time.

-   Bucket ACL

    Bucket ACLs are used to control access to buckets. The following table describes the ACLs that you can configure for a bucket.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|Public read/write|All users, including anonymous users, can read and write objects in the bucket. Fees incurred by such operations are paid by the owner of the bucket. Exercise caution when you configure this ACL.|
    |public-read|Public read|Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on objects in the bucket.|
    |private|Private|Only the bucket owner or authorized users can read and write objects in the bucket. Other users, including anonymous users cannot access the objects in the bucket without authorization.|

-   Object ACL

    Object ACLs are used to control access to objects. The following table describes the ACLs that you can configure for an object.

    |ACL|Description|Access control|
    |:--|:----------|:-------------|
    |public-read-write|Public read/write|All users, including anonymous users, can read and write the object.|
    |public-read|Public read|Only the object owner or authorized users can read and write the object. Other users, including anonymous users, can only read the object.|
    |private|Private|Only the object owner or authorized users can read and write the object. Other users, including anonymous users, cannot access the object.|
    |default|Inherited from the bucket|The ACL of the object is the same as that of the bucket that stores the object.|

    **Note:** By default, the ACL of an object is inherited from the bucket. The ACL of an object takes precedence over the ACL of the bucket that stores the object. Example: If the ACL for an object is set to public-read, all authenticated and anonymous users can read the object regardless of the bucket ACL.


For more information, see [ACL-based access control](/intl.en-US/Developer Guide/Data security/Access and control/ACL-based access control.md).

## RAM policies based on users

Resource Access Management \(RAM\) is a resource access control service provided by Alibaba Cloud. You can configure RAM policies based on the responsibilities of users. You can manage users by configuring RAM policies. For users such as employees, systems, or applications, you can control which resources are accessible. For example, you can create a RAM policy to grant users read permissions on only some objects in a bucket.

A RAM policy is in the JSON format. You can describe a RAM policy by specifying the Action, Effect, Resource, and Condition fields in the Statement field. You can configure multiple Statement fields in a RAM policy to implement flexible authorization. For more information, see [Implement access control based on RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Implement access control based on RAM policies.md).

## Bucket policies based on resources

Bucket policies provide resource-based authorization for users. Compared with RAM policies, bucket policies can be configured in the OSS console. In addition, the bucket owner can grant other users permissions to access OSS resources.

By configuring bucket policies, you can authorize RAM users under other Alibaba Cloud accounts to access your OSS resources or authorize anonymous users to access your OSS resources from specific IP addresses. For more information, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use bucket policies to authorize other users to access OSS resources.md).

## Hotlink protection based on Referer whitelists

OSS is a pay-as-you-go service. To prevent additional fees caused by unauthorized access to the data in your bucket, you can configure hotlink protection for your buckets based on the Referer field in HTTP and HTTPS requests.

You can configure a Referer whitelist to allow only requests from specified domain names or HTTP and HTTPS requests that contain the Referer header to access your OSS resources. Hotlink protection can prevent the data in public read or public read/write buckets from hotlinking to protect your legal rights. For more information, see [Configure hotlink protection](/intl.en-US/Developer Guide/Data security/Access and control/Configure hotlink protection.md).

