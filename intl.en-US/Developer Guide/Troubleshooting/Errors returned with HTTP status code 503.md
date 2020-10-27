# Errors returned with HTTP status code 503

This topic describes the causes of errors returned with HTTP status code 503 and the solutions to these errors.

## DownloadTrafficRateLimitExceeded

-   Error message: Please reduce your upload request traffic.
-   Cause: The error message returned because the downloading traffic exceeds the limit.
-   Solution: The default limit of download traffic over the Internet and internal networks is 5 Gbit/s. To adjust the limit, submit a ticket.

## UploadTrafficRateLimitExceeded

-   Error message: Please reduce your upload request traffic.
-   Cause: The error message returned because the uploading traffic exceeds the limit.
-   Solution: The default limit of upload traffic through the Internet and internal networks is 5 Gbit/s. To adjust the limit, submit a ticket.

## MetaOperationQpsLimitExceeded

-   Error message: Qps limit for the meta operation is exceeded.
-   Cause: The error message returned because the QPS exceeds the default limit.

    OSS limits the QPS for the following API operations:

    -   Operations related to services, such as GetService \(ListBuckets\)
    -   Operations related to buckets, such as PutBucket and GetBucketLifecycle
    -   Operations related to CORS, such as PutBucketCORS and GetBucketCORS
    -   Operations related to LiveChannel, such as PutLiveChannel and DeleteLiveChannel.
-   Solution: We recommend that you perform the operation again after a few seconds.

