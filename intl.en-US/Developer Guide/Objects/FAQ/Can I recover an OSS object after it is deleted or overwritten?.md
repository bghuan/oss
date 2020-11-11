# Can I recover an OSS object after it is deleted or overwritten?

The redundancy mechanism of OSS is used to recover data when server or hardware failures occur. Alibaba Cloud deletes or overwrites data after you manually delete or overwrite the data, or configure automatic deletion to delete the data. The deleted data cannot be recovered.

## Data deletion and overwriting

-   The following items are the description about user data management in [Service Terms](https://help.aliyun.com/document_detail/31821.html):
    -   You can independently manage your business data in OSS, including delete and modify data. If you manually release services or delete data, Alibaba Cloud does not retain the data.
    -   If the user business data is deleted, it cannot be recovered. You shall assume the consequences and responsibilities resulting from the deletion of such data on your own. You understand and agree that Alibaba Cloud has no obligation to continue to retain, export, or return the user business data.
-   The following content is the description about data destructibility in Service Level Agreement \([SLA](https://help.aliyun.com/document_detail/60175.html)\):

    If you manually delete data or need to delete data when your services expire, Alibaba Cloud automatically deletes disk data and clears memory on the corresponding physical server. The data cannot be recovered.


## Operations that may cause data to be deleted or overwritten

Your data may be deleted or overwritten when you use the following methods. Exercise caution when you use the methods.

-   Delete objects by using the OSS console, ossutil, ossbrowser, or SDKs. For more information, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md).
-   Upload an object that has the same name as an existing object to OSS by using the OSS console, ossutil, ossbrowser, or SDKs. The existing object is overwritten by the uploaded object.
-   Configure lifecycle rules to delete objects on a regular basis. OSS automatically deletes objects based on the lifecycle rules. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   Configure cross-region replication \(CRR\) rules for your bucket and enable **Add/Delete/Change** synchronization from the source bucket to your bucket. If objects in the source bucket are modified or deleted, the changes are synchronized to the destination bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).
-   Other users delete or overwrite your objects because access permissions on the bucket are improperly configured. For more information about access permissions, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

## How do I avoid accidental deletion and overwriting?

-   Enable versioning

    When versioning is enabled for a bucket, the objects that are deleted or overwritten are stored as previous versions in the bucket. You can recover an object to a previous version at any time. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   Use CRR to back up objects

    You can configure CRR rules for your bucket and enable **Add/Delete/Change** synchronization from your bucket to another bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

-   Configure scheduled backup

    You can use Hybrid Backup Recovery \(HBR\) to back up your objects. This allows you to recover your objects when the objects are lost. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md).

-   Configure appropriate access permissions

    Take note of the following principles to grant appropriate access permissions to other users who access your bucket:

    -   Do not use your Alibaba Cloud account to access OSS.
    -   Grant read and write permissions to different RAM users. Use a RAM user that has only read permissions or an Security Token Service \(STS\) temporary access credential to access read-only data.
    -   We recommend that you provide an STS temporary access credential to the users who need to only temporarily access your data.
    -   Grant the least but sufficient access permissions on OSS data for different businesses.
    -   Keep the credentials for data access secure, such as the password of your Alibaba Cloud account and the access credentials of a RAM user.
    For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).


