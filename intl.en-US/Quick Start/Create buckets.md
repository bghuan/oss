# Create buckets

After you activate Alibaba Cloud OSS, you must create a bucket in the OSS console to store objects.

## Use the OSS console

To create a bucket in the OSS console, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the page that appears, click **Create Bucket**.

    You can also click **Overview**. Click **Create Bucket** in the upper-right corner.

3.  In the Create Bucket dialog box that appears, configure the parameters listed in the following table.

    |Parameter|Descripton|
    |---------|----------|
    |**Bucket Name**|Set the name of the bucket. The name cannot be changed after the bucket is created. The naming conventions are as follows:     -   The bucket name must be globally unique in Alibaba Cloud OSS.
    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length. |
    |**Region**|Select the region for the bucket. The region cannot be changed after the bucket is created. To access OSS from an ECS instance over the internal network, select the region in which the ECS instance is located. For more information, see [Endpoints](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).|
    |**Storage Class**|Select the storage class for the bucket.     -   **Standard**: provides storage services featuring high performance, reliability, and availability. This storage class is suitable for frequently accessed data.
    -   **IA**: has lower storage prices and is suitable for long-term storage of data. Data of this storage class is less frequently accessed. Objects of the IA storage class must be stored for a minimum period of 30 days. Fees are incurred if objects of the IA storage class are deleted before they are stored for 30 days. Objects of the IA storage class also have a minimum billable storage of 64 KB. Objects less than 64 KB are charged as 64 KB. In addition, retrieving data of this storage class also incurs fees.
    -   **Archive**: is suitable for long-term \(at least six months\) storage of data that is infrequently accessed. The data may take up to one minute to restore before it can be read. This storage option is suitable for data such as archival data, medical images, scientific materials, and video footage.
    -   **Cold Archive**: is suitable for storing extremely cold data with ultra-long lifecycles. Such data includes data that must be retained for an extended period of time due to compliance requirements, raw data that has been accumulated over an extended period of time in the big data and AI fields, media resources that have been retained in the film and television industries, and archived videos from the online education industry.

**Note:** The Cold Archive storage class is in public preview in the Australia \(Sydney\), US \(Silicon Valley\), Germany \(Frankfurt\), Indonesia \(Jakarta\), India \(Mumbai\), China \(Hong Kong\), and Singapore region. You can contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
    |**Zone-redundant Storage**|For buckets in China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), China \(Shanghai\), and Singapore, you can select whether to enable zone-redundant storage \(ZRS\).     -   Enable: After ZRS is enabled, OSS backs up your data to three zones within the same region. If the storage class of the bucket is Standard, the objects in the bucket are standard \(ZRS\) objects by default. For more information, see [Zone-redundant storage](/intl.en-US/Developer Guide/Data security/Disaster recovery/Zone-redundant storage.md).

**Note:** This feature incurs extra costs and cannot be disabled after it is enabled. Exercise caution when you enable this feature.

    -   Disable: After ZRS is disabled, the redundancy type of the objects in the bucket is locally redundant storage \(LRS\). If the storage class of the bucket is Standard, the objects in the bucket are standard \(LRS\) objects by default. |
    |**Versioning**|Select whether to enable versioning.     -   **Enable**: When versioning is enabled for a bucket, an object that is overwritten or deleted is saved as a previous version of the object. Versioning allows you to restore objects in a bucket to any previous point in time, and protects your data from being accidentally overwritten or deleted. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
    -   **Disable**: disables versioning.
**Note:** Versioning is supported in all regions except for Japan \(Tokyo\). |
    |**Access Control List \(ACL\)**|Select the bucket ACL.     -   **Private**: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
    -   **Public Read**: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket.

**Warning:** All Internet users can access objects in the bucket. This may cause unexpected access to the data in your bucket, and cause an increase in your fees. Exercise caution when you set your bucket ACL to Public Read.

    -   **Public Read/Write**: All users, including anonymous users, can perform read and write operations on objects in the bucket.

**Warning:** All Internet users can access objects in the bucket and write data to the bucket. This may cause unexpected access to the data in your bucket, and cause an increase in your fees. If a user uploads prohibited data or information, it may affect your legitimate interests and rights. Therefore, if there are no special requirements, we recommend that you do not set your bucket ACL to Public Read/Write. |
    |**Server-side Encryption**|Select whether to enable server-side encryption.     -   **Encryption Method**: Select an encryption method for the object.
        -   **None**: Server-side encryption is disabled.
        -   **OSS-Managed**: You can use keys managed by OSS for encryption. OSS server-side encryption uses AES-256 to encrypt objects by using different data keys. AES-256 uses master keys that are regularly rotated to encrypt data keys.
        -   **KMS**: You can use a specified CMK ID or the default CMK stored in KMS to encrypt or decrypt data. For more information about KMS-based encryption, see [Implement server-side encryption with CMKs stored in KMS \(SSE-KMS\)](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

**Note:**

            -   Before you use KMS-based encryption, you must [activate KMS](https://www.alibabacloud.com/zh/products/kms).
            -   You are charged for calling API operations when you use CMKs to encrypt or decrypt data. For more information about the fees, see [KMS pricing](/intl.en-US/Pricing/Billing.md).
    -   **Encryption Algorithm**: Only AES-256 is supported.
    -   **CMK**: You can configure this parameter if you select **KMS** in the **Encryption Method** section. Parameter description:
        -   **alias/acs/oss**: The default CMK stored in KMS is used to encrypt different objects and decrypt the objects when they are downloaded.
        -   **CMK ID**: The keys generated by a specified CMK are used to encrypt different objects and the specified CMK ID is recorded in the metadata of the encrypted object. Objects are decrypted when they are downloaded by users who have decryption permissions. Before you specify a CMK ID, you must create a normal key or an [external key](/intl.en-US/User Guide/Use symmetric keys/Import key material.md) in the same region as the bucket in the [KMS console](https://kms.console.aliyun.com). |
    |**Real-time Log Query**|Select whether to enable real-time log query for OSS.     -   **Enable**: enables real-time log query for OSS. OSS uses Log Service to provide real-time OSS log queries for the last seven days free of charge. After this feature is enabled, you can query and analyze records of access to objects in OSS buckets by using the OSS console in real time. For more information, see [Real-time log query](/intl.en-US/Developer Guide/Manage logs/Real-time log query.md).
    -   **Disable**: disables real-time log query. |
    |**Scheduled Backup**|Select whether to create a scheduled backup plan to back up your OSS data by using Hybrid Backup Recovery \(HBR\).     -   **Enable**: After scheduled backup is enabled, OSS creates a backup plan to buck up data once a day and retain the backup files for one week. You can choose **Files** \> **Scheduled Backup** to view the backup plan that was created.
    -   **Disable**: creates no scheduled backup plans.
**Note:** If HBR is not activated or not authorized to access OSS, scheduled backup plans cannot be created. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md). |

4.  Click **OK**.


## Use ossutil

ossutil is a command line tool for OSS. You can use ossutil to create a bucket. For more information, see [mb](/intl.en-US/Tools/ossutil/Common commands/mb.md).

## Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development. For more information, see the following topics:

-   API operation: [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md)
-   Java SDK: [Create buckets](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)
-   Python SDK: the "Create a bucket" section in [Manage buckets](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)
-   PHP SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)
-   Go SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)
-   C SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)

For more information about SDK examples in more programming languages, see [t22258.dita\#concept\_dcn\_tp1\_kfb](/intl.en-US/SDK Reference/Introduction.md).

[Upload objects](/intl.en-US/Quick Start/Upload objects.md)

