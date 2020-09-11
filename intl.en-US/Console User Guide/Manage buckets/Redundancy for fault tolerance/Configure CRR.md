# Configure CRR

Cross-region replication \(CRR\) enables the automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. If you enable CRR, operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

This feature meets the requirements of geo-disaster recovery or data replication. Objects in the destination bucket are extra replicas of objects in the source bucket. They have the same object names, object content, and object metadata such as the created time, owner, user metadata, and object ACL.

For more information about CRR, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).

## Enable CRR

To enable CRR, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Redundancy for Fault Tolerance** \> **Cross-Region Replication**.

4.  Click **Enable**. In the Cross-Region Replication dialog box that appears, configure the parameters listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Source Region**|The region where the current bucket is located.|
    |**Source Bucket**|The name of the current bucket.|
    |**Destination Region**|Select the region where the destination bucket is located.The source and destination buckets for CRR must be located in different regions. Data cannot be synchronized between buckets located within the same region. |
    |**Destination Bucket**|Select the destination bucket.The two buckets that has CRR enabled cannot synchronize data with other buckets. If you synchronize data from Bucket A to Bucket B, neither Bucket A nor Bucket B can synchronize data with other buckets. |
    |**Acceleration Type**|You must enable transfer acceleration when you perform cross-region replication from regions outside China to regions in China. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).**Note:** Currently, transfer acceleration can be enabled only for cross-region replication from Australia \(Sydney\) to China \(Beijing\). |
    |**Applied To**|Select the source data to synchronize.     -   **All Files in Source Bucket**: synchronizes all objects from the source bucket to the destination bucket.
    -   **Files with Specified Prefix**: synchronizes the objects whose names contain the specified prefix from the source bucket to the destination bucket. You can specify up to 10 prefixes.

If you have a folder named management in the root directory of a bucket and a subfolder named abc in management. If you want to synchronize objects in the abc folder, enter management/abc as the prefix. |
    |**Object tagging**|Select **Configure Rules** and add tags \(key-value pairs\). Only objects that have the specified tags are synchronized to the destination bucket.You can configure CRR rules based on object tags only when versioning is enabled for both the source and destination buckets and **Add/Change** is selected for **Operations**. A maximum of 10 tags can be specified. For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md). |
    |**Operations**|Select the synchronization policy.     -   **Add/Change**: synchronizes only the added or changed data from the source bucket to the destination bucket.
    -   **Add/Delete/Change**: synchronizes all data changes such as the creation, overwriting, and deletion of objects from the source bucket to the destination bucket.
For more information about CRR for objects that have versioning configured, see [Replication in special scenarios](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication in specific scenarios.md). |
    |**Replicate Historical Data**|Specify whether to synchronize historical data before you enable CRR.    -   **Yes**: synchronizes historical data to the destination bucket.

**Note:** When historical data is synchronized, objects in the source bucket may overwrite objects in the destination bucket if these objects have the same names. To avoid overwriting objects that have the same names, we recommend that you enable versioning for the source and destination buckets.

    -   **No**: synchronizes only objects that you want to upload or update after CRR is enabled to the destination bucket. |

5.  Click **OK**.

    -   After you configure CRR rules, you cannot edit or delete CRR rules.
    -   After the configuration is complete, the incremental data is synchronized three to five minutes after CRR is enabled. If you select Replicate Historical Data, the historical data is synchronized 90 minutes after CRR is enabled. You can view the replication details after CRR is enabled.
    -   In CRR, data is asynchronously \(near real-time\) replicated. It takes several minutes to several hours for the data to be replicated to the destination bucket based on the amount of data.

## Disable CRR

You can click **Disable** to disable CRR.

![sync](../images/p135995.png)

After CRR is disabled, the replicated data is stored in the destination bucket, and the incremental data from the source bucket is not replicated to the destination bucket.

