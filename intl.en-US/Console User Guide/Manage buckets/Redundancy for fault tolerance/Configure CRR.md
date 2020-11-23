# Configure CRR

Cross-region replication \(CRR\) enables the automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. If you enable CRR, operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

This feature meets the requirements of geo-disaster recovery or data replication. Objects in the destination bucket are extra replicas of objects in the source bucket. They have the same object names, object content, and object metadata such as the creation time, owner, user metadata, and object ACL.

For more information about CRR, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).

## Enable CRR

To enable CRR, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Redundancy for Fault Tolerance** \> **Cross-Region Replication**.

4.  Click **Enable**. In the Cross-Region Replication pane, configure the parameters listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Source Region**|The region where the current bucket is located.|
    |**Source Bucket**|The name of the current bucket.|
    |**Destination Region**|Select the region where the destination bucket is located.The source and destination buckets for CRR must be located in different regions. Data cannot be synchronized between buckets located within the same region. |
    |**Destination Bucket**|Select the destination bucket to synchronize data.The two buckets that have CRR enabled cannot synchronize data with other buckets. If you synchronize data from Bucket A to Bucket B, neither Bucket A nor Bucket B can synchronize data with other buckets. |
    |**Acceleration Type**|You must enable transfer acceleration when you perform CRR in regions in mainland China and regions outside mainland China. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).|
    |**Applied To**|Select the source data to synchronize.     -   **All Files in Source Bucket**: synchronizes all objects from the source bucket to the destination bucket.
    -   **Files with Specified Prefix**: synchronizes the objects whose names contain the specified prefix from the source bucket to the destination bucket. You can specify up to 10 prefixes.

For example, if you have a folder named management/ in the root folder of a bucket and a subfolder named abc/ in management/, when you want to synchronize objects in the abc/ subfolder, enter management/abc/ as the prefix. |
    |**Object Tagging**|Objects that have the specified tags are synchronized to the destination bucket. Select **Configure Rules** and add tags \(key-value pairs\). You can add up to 10 tags.When you set this parameter, ensure that the following conditions are met:

    -   Object tags are configured. For more information, see [Configure object tagging](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object tagging.md).
    -   Versioning is enabled for the source and destination buckets.
    -   **Add/Change** is set to Operations.
    -   The source region is China \(Hangzhou\). The destination region is any region except China \(Hangzhou\). Or the source region is Australia \(Sydney\), and the destination region is any region except mainland China regions or Australia \(Sydney\). |
    |**Operations**|Select the synchronization policy.     -   **Add/Change**: synchronizes only the added or changed data from the source bucket to the destination bucket.
    -   **Add/Delete/Change**: synchronizes all data changes such as the creation, overwriting, and deletion of objects from the source bucket to the destination bucket.
For more information about CRR for objects that have versioning configured, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |
    |**Replicate Historical Data**|Specify whether to synchronize historical data before you enable CRR.    -   **Yes**: synchronizes historical data to the destination bucket.

**Note:** When historical data is synchronized, objects in the source bucket may overwrite objects in the destination bucket if these objects have the same names. To avoid overwriting objects that have the same names, we recommend that you enable versioning for the source and destination buckets.

    -   **No**: synchronizes only objects that you want to upload or update to the destination bucket after CRR rules take effect. |
    |**KMS-based Encryption**|Use SSE-KMS to encrypt data. OSS uses a specified customer master key \(CMK\) to generate different keys to encrypt objects that you want to replicate to the destination bucket.    -   **CMK ID**: the CMK ID used to encrypt objects replicated to the destination bucket, which is to specify the CMK ID for the destination object.

To use a CMK to encrypt objects, you must create a CMK in the same region as the destination bucket. For more information, see [Manage CMKs]().

You are charged for calling API operations when you use CMKs to encrypt or decrypt data.

For more information about CRR for objects that have server-side encryption configured, see [Cross-region replication in specific scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md).

    -   **RAM Role Name**: Select New RAM Role or AliyunOSSRole to encrypt data for the destination object by using CMKs.
        -   **New RAM Role**: A new RAM role is created by OSS to encrypt data for the destination object by using CMKs. The format is kms-replication-source bucket name-destination bucket name.
        -   **AliyunOSSRole**: The AliyunOSSRole role is used to encrypt data for the destination object by using CMKs. If AliyunOSSRole does not exist, it is created. |

5.  Click **OK**.

    -   After you configure CRR rules, you cannot edit or delete CRR rules.
    -   After the configuration is complete, the incremental data is synchronized three to five minutes after CRR is enabled. If you select Replicate Historical Data, the historical data is synchronized 90 minutes after CRR is enabled. You can view the replication details after CRR is enabled.
    -   In CRR, data is asynchronously \(near real-time\) replicated. It takes several minutes to several hours for the data to be replicated to the destination bucket based on the amount of data.

## Disable CRR

You can click **Disable** to disable CRR.

![sync](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4658906061/p135995.png)

After CRR is disabled, the replicated data is stored in the destination bucket, and the incremental data from the source bucket is not replicated to the destination bucket.

