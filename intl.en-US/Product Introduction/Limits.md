# Limits

This topic describes the performance metrics and limits of OSS.

The following table describes the limits and performance metrics of OSS.

|Item|Limit|
|:---|:----|
|Bandwidth|Default bandwidth limit: 10 Gbit/s in mainland China regions and 5 Gbit/s in regions outside mainland China. When this limit is reached, requests are throttled.**Note:** When requests are throttled, the response header contains `x-oss-qos-delay-time: number`. The returned `number` indicates the time period during which requests are throttled. Unit: ms.

-   For an upload request, the exact time period during which requests are throttled is returned.
-   For a download request, the estimated time period during which requests are throttled is returned. The time is estimated based on the throttling status and the object size.

If you require a greater bandwidth \(10 Gbit/s to 100 Gbit/s\) or queries per second \(QPS\), for example, if you have offline big data processing requirements, contact the[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|QPS|The limit of the total QPS for a single account is 10,000. The actual values that can be achieved are different in the different read and write modes:-   Sequential read/write: 2,000

If you upload a large number of objects with sequential prefixes such as timestamps and letters in the object names, multiple object indexes may be stored in a single partition. If excessive requests are sent to query these objects, the responsiveness may be slow. We recommend that you do not upload a large number of objects by using sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Non-sequential read/write: 10,000

If you require a greater QPS, contact the[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|Bucket|-   You can use an Alibaba Cloud account to create up to 100 buckets in the same region.
-   After a bucket is created, its name, region, and storage class cannot be modified.
-   OSS does not impose limits on the capacity of a single bucket. |
|Archive|It takes about one minute to restore data from the frozen status to the readable status.|
|Upload objects|-   Objects larger than 5 GB cannot be uploaded by using the following methods: console upload, [simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md), [form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md), and [append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md). Objects larger than 48.8 TB cannot be uploaded by using [multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md).
-   If you upload an object that has the same name of an existing object in OSS, the new object overwrites the existing object. |
|Object management|When you change the storage class of objects by calling the CopyObject operation in the console, the object size cannot exceed 1 GB.|
|Object deletion|-   Deleted objects cannot be recovered.
-   You can delete up to 100 objects at a time from the console. To delete more than 100 objects at a time, you must call API operations or use SDKs. |
|Domain name binding|-   You must complete real-name verification for your account on the Alibaba Cloud official website.
-   For domain names bound to buckets located within mainland China regions, you must apply for an ICP filing at the Ministry of Industry and Information Technology \(MIIT\).
-   A single bucket can be bound to up to 100 domain names, but a domain name can be bound only to a single bucket. |
|Lifecycle|You can configure up to 1,000 lifecycle rules for each bucket.|
|Back-to-origin rules|-   You can configure up to 20 back-to-origin rules for each bucket.
-   In regions within mainland China and China \(Hong Kong\), the default QPS of mirroring-based back-to-origin is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin is 1,000, and the default bandwidth is 1 Gbit/s. |
|Image Processing \(IMG\)|-   Limits on images
    -   Source images
        -   Only JPG, PNG, BMP, GIF, WebP, and TIFF objects are supported.
        -   The object size cannot exceed 20 MB.
        -   If you use image rotation, the width or height of the image cannot exceed 4,096 pixels.
        -   Each side of the image cannot exceed 30,000 pixels.
        -   The source image cannot exceed 250 million pixels in total.
    -   Resized images
        -   The product of dimensions cannot exceed 4,096 × 4,096 pixels.
        -   Each side of the image cannot exceed 4,096 pixels.
-   Limits on image styles

You can create up to 50 image styles in each bucket. If you require more than 50 styles, contact the[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |

