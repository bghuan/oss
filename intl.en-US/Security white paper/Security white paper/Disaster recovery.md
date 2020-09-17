# Disaster recovery

OSS supports zone-redundant storage and cross-region replication to provide disaster recovery capabilities for data centers in a same region or across multiple regions.

## Zone-redundant storage \(ZRS\)

ZRS distributes user data across three zones within the same region. Even if one zone becomes unavailable, the data is still accessible. The ZRS feature can provide data durability \(designed for\) of 99.9999999999% \(twelve 9's\) and service availability of 99.995%.

ZRS offers data center-level disaster recovery capabilities. When a data center is unavailable due to network disconnection, power outage, or other disaster events, OSS can provide highly consistent services. This way, the service is not interrupted and data is not lost during the failover. This meets the strict requirements of key business systems that the recovery time objective \(RTO\) and the recovery point objective \(RPO\) must be zero.

ZRS supports the Standard and Infrequent Access \(IA\) storage classes. The following table compares the two storage classes from different dimensions.

|Index|Standard|IA|
|:----|:-------|:-|
|Data durability \(designed for\)|99.9999999999% \(twelve 9's\)|99.9999999999% \(twelve 9's\)|
|Service availability|99.995%|None|
|Service availability \(designed for\)|None|99.995%|
|Minimum billable size of objects|Actual size of objects|64 KB|
|Minimum storage period|N/A|30 days|
|Data retrieval fees|None|Based on the size of retrieved data. Unit: GB.|
|Data access|Real-time access with low latency \(within milliseconds\)|Real-time access with low latency \(within milliseconds\)|
|Image Processing \(IMG\)|Supported|Supported|

For more information, see [Zone-redundant storage](/intl.en-US/Developer Guide/Data security/Disaster recovery/Zone-redundant storage.md).

## Cross-region replication

Cross-region replication \(CRR\) enables the automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket.

CRR can meet the following business requirements:

-   Compliance requirements: Although OSS stores multiple replicas of each object in physical disks, replicas must be stored at a distance from each other to comply with regulations. CRR allows you to replicate data between geographically distant OSS data centers to satisfy these compliance requirements.
-   Minimum latency: You have users who are located in two geographical locations. To minimize the latency when the users access objects, you can maintain replicas of objects in OSS data centers that are geographically closer to these users.
-   Data backup and disaster recovery: You have high requirements for data security and availability, and want to explicitly maintain replicas of all written data in a second data center. If one OSS data center is damaged in a catastrophic event such as an earthquake or a tsunami, you can use backup data from the other data center.
-   Data replication: For business reasons, you may need to migrate data from one OSS data center to another data center.
-   Operational reasons: You have compute clusters deployed in two different data centers that need to analyze the same group of objects. You can choose to maintain object replicas in these regions.

CRR can meet your requirements on geo-disaster recovery and data replication. Objects in the destination bucket are exact replicas of those in the source bucket. They have the same object names, versioning information, object content, and object metadata such as the creation time, owner, user metadata, and object ACLs. CRR can replicate objects that are not encrypted and objects that are encrypted by using SSE-KMS or SSE-OSS at the server side.

For more information, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).

