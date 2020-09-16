# Zone-redundant storage

OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data will still be accessible. The zone-redundant storage \(ZRS\) feature is designed to provide 99.9999999999 \(twelve 9s\) durability and 99.995% availability of data over a given year.

ZRS offers data center-level disaster recovery capabilities. When a data center is unavailable due to network disconnection, power outage, or other disaster events, OSS can provide highly consistent services. This way, the service is not interrupted and data is not lost during the failover. This meets the strict requirements of key business systems that the recovery time objective \(RTO\) and the recovery point objective \(RPO\) must be zero.

**Note:**

-   ZRS is available in the Singapore, China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), and China \(Shanghai\) regions. This feature will be available in other regions.
-   ZRS incurs extra costs. For more information about pricing, see [Object Storage Service Pricing](https://www.alibabacloud.com/zh/product/oss/pricing).

## Supported storage classes

ZRS supports the Standard and Infrequent Access \(IA\) storage classes. The following table compares the two storage classes from different dimensions.

|Index|Standard|IA|
|:----|:-------|:-|
|Data durability \(designed for\)|99.9999999999% \(twelve 9's\)|99.9999999999% \(twelve 9's\)|
|Service availability|99.95%|99.50%|
|Service availability \(designed for\)|99.995%|99.995%|
|Minimum billable size of objects|Actual size of objects|64 KB|
|Minimum storage period|N/A|30 days|
|Data retrieval fee|N/A|Based on the size of retrieved data. Unit: GB.|
|Data access|Real-time access with latency within several milliseconds|Real-time access with latency within several milliseconds|
|IMG|Supported|Supported.|

## Enable ZRS

You can configure ZRS for a bucket only when you create the bucket. You cannot enable ZRS for an existing bucket.

If you want to replicate data from an existing bucket to a ZRS-enabled bucket, use migration tools such as ossimport and SDKs.

