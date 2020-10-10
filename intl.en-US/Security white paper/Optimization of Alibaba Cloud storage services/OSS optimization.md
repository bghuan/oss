# OSS optimization

OSS provides various storage management features to help you optimize your storage performance and cost.

You can analyze the access mode of your data and configure [lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md) to automatically convert data that is infrequently accessed to lower-cost storage classes. To manage data stored in OSS more efficiently, you can add tags to objects to classify them and use tags as filtering conditions in lifecycle rules.

[Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md) helps you understand the status of objects in your buckets and simplify and speed up workflows and big data tasks. The bucket inventory feature scans objects in your bucket on a weekly basis, generates an inventory list in the CSV format, and stores the list as an object in the specified bucket. You can specify object metadata to be exported to the inventory list, such as object size and encryption status.

[OSS monitoring service](/intl.en-US/Developer Guide/Monitoring service/Overview.md) provides metrics to measure the running status and performance of the system. The monitoring service also provides a custom alert service to help you track requests, analyze usage, collect statistics for business trends, and discover and diagnose system problems in a timely manner.

By using the information provided by the preceding storage management features, you can configure lifecycle rules to convert data that is infrequently accessed to low-cost storage classes to realize significant cost savings. For example, you can save up to 40% of your storage cost by converting the storage class of your data from Standard to IA and save up to 70% of your storage cost by converting the storage class of expired data to Archive.

The following table compares the monthly storage cost \(including data retrieval fees\) of 1 PB of Standard data and IA data. According to the data in the table, if 10% of your data is accessed every month, the IA storage saves 31% of the cost. If 50% of your data is accessed every month, the IA storage saves 20% of the cost. Even if 100% of your data is accessed every month, the IA storage still saves 6% of the cost.

|Total size of data|Percentage of accessed data|Cost of Standard storage \(CNY\)|Cost of IA storage \(CNY\)|Saved cost|
|------------------|---------------------------|--------------------------------|--------------------------|----------|
|1 PB|10%|125,829|87,294|31%|
|1 PB|50%|125,829|100,925|20%|
|1 PB|100%|125,829|117,965|6%|

To further optimize storage and data retrieval costs, OSS provides the [OSS Select](/intl.en-US/Developer Guide/Objects/Manage files/SelectObject.md) feature. In general, an object in OSS is accessed as a whole regardless of the object size. OSS Select allows you to use simple SQL statements to retrieve objects. Therefore, your application does not need to use computing resources to scan and filter the data in objects. OSS Select can increase query performance by four times and reduce query cost by 80%. OSS also supports the retrieval of IA and Archive objects. Therefore, you can find the data to analyze without performing data retrieval operations. By using OSS Select, you can reduce query cost and obtain more data insights.

