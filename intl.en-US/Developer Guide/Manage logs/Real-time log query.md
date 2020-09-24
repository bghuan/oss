# Real-time log query

When you access OSS, a large number of access logs are generated. Real-time log query combines OSS with Log Service. This feature allows you to query and collect statistics for OSS access logs and audit access to OSS by using the OSS console, track exceptions, and troubleshoot problems. Real-time log query enables you to be more efficient and make informed decisions.

## Comparison between real-time log query and logging

-   Real-time log query:
    -   Pushes logs to Log Service within three minutes and allows you to view real-time logs in the OSS console.
    -   Provides the log analysis service and typical analysis reports so that you can easily query data.
    -   Allows you to query and analyze raw logs in real time and filter logs by bucket, object name, API operation, or time.
-   Logging:
    -   Allows you to enable logging for a bucket, after which OSS generates log objects in accordance with a predefined naming convention. This way, hourly access logs are written to the specified bucket as objects.
    -   Allows you to use Alibaba Cloud Data Lake Analytics or build a Spark cluster to analyze access logs.
    -   Allows you to configure lifecycle rules for the specified destination bucket to convert the storage class of the log objects to Archiveor Cold Archive. This way, these log objects can be retained for a long time.

## Configuration method

Console: [Configure real-time log query](/intl.en-US/Console User Guide/Manage buckets/Manage logs/Configure real-time log query.md)

## Query method

The real-time log query feature provides the following query methods:

-   Query raw logs

    You can specify the time range and query statement to query logs in real time and perform the following operations:

    -   Analyze the distribution of a specified field such as an API operation within a specified time range.
    -   Filter by field to view required access records. For example, filter the object deletion operations for the last day by bucket, object name, or API operation name, and query the deletion time and IP address.
    -   Collect statistics for OSS access records such as the page view \(PV\), unique visitor \(UV\), or maximum latency of a bucket within a specified time range.
-   Use Dashboard

    Dashboard allows you to view four immediately available reports.

    -   **Access Center**: displays the overall operating status including the PV, UV, traffic, and distribution of access over the Internet.
    -   **Audit Center**: displays statistics for object operations including read, write, and delete operations on objects.
    -   **Operation Center**: displays statistics for access logs including the number of requests and distribution of failed operations.
    -   **Performance Center**: displays statistics for performance including the performance of downloads and uploads over the Internet, the performance of transmission over different networks or with different object sizes, and the list of differences between stored and downloaded objects.
-   Use the Log Service console

    You can view OSS access logs in the [Log Service console](https://sls.console.aliyun.com/). For more information, see [OSS access logs](/intl.en-US/Data Collection/Cloud product collection/OSS access logs/Usage notes.md).


## Billing method

-   Real-time log query allows you to query logs from the past seven days free of charge. If the log retention time that you set is longer than seven days, Log Service charges the fees for the excess days. Extra fees are charged when you read data from or write data to Log Service over the Internet.
-   Log Service allows you to store 900 GB of logs \(equivalent to 900 million 1-KB log entries\) per day free of charge, and charges fees for excess logs.

For more information about the billing standards, see [Pay-as-you-go](/intl.en-US/Pricing/Pay-as-you-go.md).

