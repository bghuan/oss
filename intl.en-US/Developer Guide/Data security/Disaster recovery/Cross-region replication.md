# Cross-region replication

Cross-region replication \(CRR\) enables the automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

CRR can meet your requirements on geo-disaster recovery and data replication. Objects in the destination bucket are exact replicas of those in the source bucket. They have the same object names, versioning information, object content, and object metadata such as the creation time, owner, user metadata, and object ACLs.

## Implementation modes

You can configure CRR in the OSS console or by using OSS SDK for Java:

-   [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md) in the console
-   [Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Cross-region replication.md)

## Scenarios

CRR can be configured for a wide range of scenarios:

-   Compliance requirements

    Although OSS stores multiple replicas of each object in physical disks, replicas must be stored at a distance from each other to comply with regulations. CRR allows you to replicate data between geographically distant OSS data centers to satisfy the compliance requirements.

-   Minimum latency

    You have customers located in two geographical locations. To minimize the latency when the customers access objects, you can maintain replicas of objects in OSS data centers that are geographically closer to the customers.

-   Data backup and disaster recovery

    You have high requirements for data security and availability, and want to explicitly maintain replicas of all written data in a second data center. If one OSS data center is damaged in a catastrophic event such as an earthquake or a tsunami, you can use backup data from the other data center.

-   Data migration

    For business reasons, you may need to migrate data from one OSS data center to another data center.

-   Operational reasons

    You have compute clusters that are deployed in two data centers to analyze the same group of objects. You can choose to maintain object replicas in the two regions.


## Capabilities

CRR supports the synchronization between buckets that have different names. If two buckets are in different regions, you can use this feature to synchronize data from the source bucket to the destination bucket in real time. This feature offers the following capabilities:

-   Real-time data synchronization

    This capability monitors data addition, deletion, and modification in real time and synchronizes these changes to the destination bucket. Data in an object up to 2 MB in size is synchronized within 10 minutes to ensure data consistency between the source and the destination buckets.

-   Historical data migration

    This capability synchronizes historical data from the source bucket to the destination bucket. Two identical data replicas are provided.

-   Real-time display of the synchronization progress

    This capability displays the last synchronization time for real-time data synchronization and the percentage of synchronization for historical data migration.

-   Mutual synchronization

    You can configure data synchronization between Bucket A and Bucket B to enable mutual data synchronization.

-   Versioning

    If versioning is enabled, the object version information is consistent between the source and destination buckets. If write synchronization of add and change operations is used as the synchronization mode, changes are not synchronized to the destination bucket when an object version in the source bucket is deleted. However, the delete marker is synchronized from the source bucket to the destination bucket.

-   Transfer acceleration

    OSS allows you to use transfer acceleration to speed up data transfer when you replicate data between regions in mainland China and regions outside mainland China. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

    **Note:** Mainland China refers to regions in China except China \(Hong Kong\).


## Limits

When you use CRR, take note of the following items:

-   Supported regions
    -   Tagging is supported when CRR is configured for bucket synchronizations only from Australia \(Sydney\) to regions outside mainland China.
    -   You must enable transfer acceleration when you perform CRR between regions in mainland China and regions outside mainland China.
-   Billing
    -   When CRR is enabled, CRR traffic is generated when you replicate objects between buckets in the source and destination regions. You are charged for the traffic when you use CRR. For more information, see [Traffic fees](/intl.en-US/Pricing/Billing items and methods/Traffic fees.md).
    -   Each time an object is synchronized, OSS counts the number of requests and calculates the charges on a pay-as-you-go basis. For more information, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md).
    -   You are charged for transfer acceleration if you enable transfer acceleration. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).
-   Replication time

    CRR is an asynchronous \(near real-time\) process. Based on the size of the data, it takes several minutes to several hours to replicate data from the source bucket to the destination bucket.

-   Operation limits
    -   Synchronization is supported only between two buckets across different regions. You cannot synchronize data between buckets within the same region.
    -   You can synchronize data only between two buckets that are in the same versioning state.
    -   You cannot change the versioning status of the source and destination buckets that are being synchronized.
    -   You can perform operations on buckets that are being synchronized at the same time. However, an object that is replicated from the source bucket may overwrite an object that has the same name in the destination bucket.
    -   CRR can be applied only to two buckets that are not synchronized with any other buckets. For example, if you use CRR to synchronize data from Bucket A to Bucket B, you are not allowed to synchronize data from Bucket A to Bucket C, unless you delete the CRR configurations to synchronize data from Bucket A to Bucket B. Similarly, if you synchronize data from Bucket A to Bucket B, you are not allowed to synchronize data from Bucket C to Bucket B.

