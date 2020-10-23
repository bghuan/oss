# Transfer acceleration

Alibaba Cloud OSS provides transfer acceleration to improve data transfer speeds. Transfer acceleration enables enterprises to deploy their business in more Alibaba Cloud regions and improve user experience. OSS provides transfer acceleration to accelerate the upload and download of OSS objects over the Internet. Transfer acceleration provides an end-to-end acceleration solution by combining smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations.

OSS uses data centers distributed around the globe to implement transfer acceleration. When a data transfer request is sent, it is resolved and routed over an optimal network path and protocol to the data center where your bucket resides.

## Scenarios

OSS transfer acceleration can be used in the following scenarios to accelerate access and improve user experience:

-   Accelerates remote data transfer

    For example, forums and online collaboration tools that have users all across the globe can store data in OSS. However, upload and download speeds vary from region to region, which results in inconsistent access experience. In this case, OSS transfer acceleration can be used to allow users from different regions to transfer data over the optimal network based on their regions. This feature accelerates data transfer and improves access experience for users across different regions.

-   Accelerates uploads and downloads of objects of gigabytes or terabytes in size

    When you upload or download large objects in remote regions over the Internet, you can enable transfer acceleration to accelerate data transfer. Transfer acceleration uses optimal Internet route selection and protocol stack tuning to significantly reduce data transfer timeouts in remote regions. [Multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) allows you to implement resumable upload from the position where the upload failed. You can integrate multipart upload and transfer acceleration to upload and download large objects in remote regions.

-   Accelerates non-static and non-hotspot data download

    For example, user experience is a driving factor for product competitiveness and customer retention in apps that require high data download speeds, such as photo management apps, games, e-commerce apps, enterprise portal websites, and financial apps. Obtaining comments on social networking apps also requires high download speeds. OSS transfer acceleration is a service designed to accelerate OSS uploads and downloads. You can enable transfer acceleration to maximize bandwidth utilization, which accelerates data transfer.


## Instructions

When transfer acceleration is enabled for a bucket, two public endpoints become available for the bucket.

-   The following accelerate endpoints are available:

    -   Global accelerate endpoint: The URL is oss-accelerate.aliyuncs.com. Transfer acceleration access points are distributed across the world. You can use this endpoint to accelerate access to buckets all over the world.
    -   Accelerate endpoint outside mainland China: The URL is oss-accelerate-overseas.aliyuncs.com. Transfer acceleration access points are distributed across the world except mainland China. We recommend that you use accelerate endpoints outside mainland China only when you need to bind a custom domain name for which you have not applied for an ICP filing to a bucket outside mainland China, and use the global accelerate endpoint in other scenarios. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).
    You can access data in OSS by using an accelerate endpoint to accelerate data transfer. For example, a bucket named test is located in the US \(Silicon Valley\) region. An object named 123.jpg is stored in the root folder of the bucket. When you access OSS resources by using a browser, you can use the http://test.oss-accelerate.aliyuncs.com/123.jpg accelerate endpoint to accelerate data transfer.

-   Regular endpoint: The format is oss-region.aliyuncs.com. This endpoint can be used when you do not need transfer acceleration. For more information about regular endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

**Note:** We recommend that you use the OSS internal endpoint to access an OSS bucket from an ECS instance in the same region. For example, assume that a bucket and an ECS instance are located in the China \(Shanghai\) region. We recommend that you use oss-cn-shanghai-internal.aliyuncs.com to access OSS from the ECS instance.

In this example, ossutil is used to test the actual acceleration effect. The following figure shows the results.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2295688951/p57059.png)

You can compare the speeds of access from different regions when the accelerate and regular endpoints are used. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).

## Implementation modes

For more information about OSS transfer acceleration, see [Configure transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Configure transfer acceleration.md). When transfer acceleration is enabled, you can use the global accelerate endpoint to implement transfer acceleration for access to data stored in OSS by using one of the following methods:

-   Browser

    When you use a browser to access data stored in OSS, replace the endpoint in the object URL with an accelerate endpoint. For example, replace https://test.oss-cn-shenzhen.aliyuncs.com/myphoto.jpg with https://test.oss-accelerate.aliyuncs.com/myphoto.jpg. If the object ACL is private, you must add signature information to the object URL.

    **Note:** To accelerate access to a bucket that has transfer acceleration enabled and a custom domain name bound, configure CNAME to map your custom domain name to the accelerate endpoint. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

-   ossutil

    When you use ossutil to access data stored in OSS, replace the endpoint in the configuration file with an accelerate endpoint. Alternatively, add `-e oss-accelerate.aliyuncs.com` to each command to perform an operation. For more information about how to configure the endpoint when you use ossutil, see [ossutil](/intl.en-US/Tools/ossutil/Common commands/config.md).

-   ossbrowser

    When you use ossbrowser to access data stored in OSS, set the endpoint to Customize, and specify an accelerate endpoint. For more information about how to configure the endpoint when you use ossbrowser, see [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md).

-   SDK

    When you use SDKs for different languages to access OSS, set the endpoint to an accelerate endpoint. The following code provides an example on how to use OSS SDK for Java to perform simple upload and download:

    -   Simple upload

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Upload the object to OSS. <yourLocalFile> consists of the local file path and a file name with extension. Example: /users/local/myfile.txt.
        ossClient.putObject("<yourBucketName>", "<yourObjectName>", new File("<yourLocalFile>"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    -   Simple download

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Download the object to a local file. If the specified local file exists, the downloaded object replaces the local file. Otherwise, the local file is created.
        ossClient.getObject(new GetObjectRequest(bucketName, objectName), new File("<yourLocalFile>"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    For more information about SDK examples, see [Introduction](/intl.en-US/SDK Reference/Introduction.md).


## Usage notes

-   You must complete real-name registration on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page before you enable transfer acceleration for a bucket.
-   When you enable or disable transfer acceleration, the configurations take effect in 30 minutes.
-   Access is accelerated only when you use the accelerate endpoint to access a bucket that has transfer acceleration enabled.
-   When the endpoint of your tool is set to an accelerate endpoint, you can perform operations only on the bucket that has transfer acceleration enabled.
-   When transfer acceleration is enabled, other endpoints function normally. You can switch between endpoints at any time.
-   Transfer acceleration applies only to acceleration of access over the Internet.
-   For data transfer security reasons, HTTP used in the actual transfer endpoint is replaced with HTTPS when the peer end receives the request. Therefore, if you access OSS over HTTPS, the actual accelerate endpoint-based URL uses HTTPS. If you access OSS over HTTP, HTTP used in the actual accelerate endpoint-based URL may be changed to HTTPS.
-   This feature is available for all regions except China \(Heyuan\), China \(Ulanqab\), and UAE \(Dubai\).
-   Additional fees are charged based on traffic when you use transfer acceleration. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

## FAQ

-   What are the differences between transfer acceleration and Alibaba Cloud CDN?

    |Item|Description|Use scenario|
    |----|-----------|------------|
    |Transfer acceleration|Transfer acceleration combines smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations to provide an end-to-end acceleration solution.|    -   Accelerates object uploads.
    -   Accelerates remote object uploads and downloads.
    -   Accelerates uploads and downloads of large objects.
    -   Accelerates dynamic object updates and non-hotspot object downloads. |
    |Alibaba Cloud CDN|Alibaba Cloud CDN uses OSS buckets as origins and distributes the content from the origins to edge nodes. When you read data, the cached object is obtained from an optimal node to accelerate downloads.|Accelerates downloads when a large number of users in the same region concurrently download the same static file.|

-   How do I implement transfer acceleration based on a custom domain name?

    After you bind a custom domain name to OSS, you can map the CNAME to the accelerate endpoint. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).


