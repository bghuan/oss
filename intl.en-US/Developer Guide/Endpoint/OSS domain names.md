# OSS domain names

OSS assigns domain names for each bucket. This topic describes the format and use of OSS domain names.

## Format

All requests except the GetService request that are sent to OSS include third-level domains. The third-level domains contain the bucket information.

OSS domain names are in the format of BucketName.Endpoint. BucketName indicates the name of your bucket. Endpoint indicates the domain name used to access the region where your bucket is located.

OSS provides internal endpoints, public endpoints, and accelerate endpoints for regions. For example, the following endpoints are used to access the China \(Hangzhou\) region:

-   Public endpoint: https://oss-cn-hangzhou.aliyuncs.com
-   Internal endpoint: https://oss-cn-hangzhou-internal.aliyuncs.com
-   Global accelerate endpoint: https://oss-accelerate.aliyuncs.com
-   Accelerate endpoint outside mainland China: https://oss-accelerate-overseas.aliyuncs.com

You can use the internal endpoint and public endpoint to access the OSS resources in a region without additional configurations. To use the accelerate endpoint to access an OSS bucket in a region, you must enable transfer acceleration for the bucket. For more information, see [Configure transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Configure transfer acceleration.md).

**Note:**

-   OSS provides services by using HTTP RESTful APIs. You need to use different endpoints to access resources in different regions.
-   For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
-   You can also replace a public endpoint with a custom domain name to access OSS resources. For more information, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md) or [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

## Use a public endpoint to access OSS

By using a public endpoint, you can access OSS over the Internet. OSS allows you to upload or write data to OSS over the Internet free of charge. Fees are incurred when you download or read data from OSS over the Internet.

**Note:** For more information about OSS pricing and billing, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss#pricing) and [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

You can use one of the following methods to access OSS over the Internet:

-   Method 1: Use a URL that includes information about OSS resources. The format of the URL:

    ```
    <Schema>://<Bucket>. <Public endpoint>/<Object> 
    ```

    -   Schema: HTTP or HTTPS.
    -   Bucket: the name of the OSS bucket.
    -   Public endpoint: the domain name used to access the region where the bucket is located over the Internet. For more information about the endpoints used to access each region, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
    -   Object: the path to access the OSS object.
    Example: If you want to access an object whose path is example/example.txt in a bucket named examplebucket over the Internet and the bucket is located in the China \(Hangzhou\) region, you can use the following URL: https://examplebucket.oss-cn-hangzhou.aliyuncs.com/example/example.txt.

    **Note:**

    -   This domain name is valid only when the object ACL is public or public read. If the object ACL is private, you must add signature information to the URL.
    -   By default, the path of an object must be included in the OSS domain name. If you use a bucket domain name such as examplebucket.oss-cn-hangzhou.aliyuncs.com to access an object, an error is displayed. To access OSS by using a bucket domain name, configure static website hosting. For more information, see [Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md).
    You can also add the object URL to an HTML file:

    ```
    <img src="https://examplebucket.oss-cn-hangzhou.aliyuncs.com/example/example.png" />
    ```

-   Method 2: Configure a public endpoint by using an OSS SDK.

    An OSS SDK generates an endpoint by concatenating each operation. A different endpoint is generated for operations on a bucket located in a different region.

    The following code provides an example on how to configure an endpoint by using OSS SDK for Java when OSSClient is created to perform operations on a bucket located in the China \(Hangzhou\) region:

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```


## Use an internal endpoint to access OSS

By using an internal endpoint, you can access OSS over the internal network, which is used for communications between Alibaba Cloud services located within the same region. For example, you can access OSS from an ECS instance over the internal network when OSS and the ECS instance are located within the same region. The inbound and outbound traffic over the internal network is free of charge. However, you are charged for requests that you send.

You can use one of the following methods to access OSS over the internal network:

-   Method 1: Use a URL that includes information about OSS resources. The format of the URL:

    ```
    <Schema>://<Bucket>. <Internal endpoint>/<Object> 
    ```

    -   Schema: HTTP or HTTPS.
    -   Bucket: the name of the OSS bucket.
    -   Internal endpoint: the domain name used to access the region where the bucket is located from an ECS instance in the same region over the internal network. For more information about the endpoints used to access each region, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
    -   Object: the path to access the OSS object.
    Example: If you want to access an object whose path is example/example.txt in a bucket named examplebucket over the internal network and the bucket is located in the China \(Hangzhou\) region, you can use the following URL: https://examplebucket.oss-cn-hangzhou-internal.aliyuncs.com/example/example.txt.

-   Method 2: Configure an internal endpoint by using an OSS SDK to access OSS from an ECS instance.

    The following code provides an example on how to configure an internal endpoint by using OSS SDK for Java. The code is used to perform operations on a bucket located in the China \(Hangzhou\) region:

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou-internal.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```

    You can use an internal endpoint to access OSS from an ECS instance over the internal network when OSS and the ECS instance are located within the same region. If OSS and the ECS instance are located in different regions, you cannot use the internal endpoint to access OSS from the ECS instance over the internal network. For example, you have two buckets in OSS and you have purchased an ECS instance located in the China \(Beijing\) region.

    -   One bucket named beijingres is located in the China \(Beijing\) region. You can use https://beijingres.oss-cn-beijing-internal.aliyuncs.com to access the resources in beijingres from the ECS instance.
    -   The other bucket named qingdaores is located in the China \(Qingdao\) region. The internal endpoint https://qingdaores.oss-cn-qingdao-internal.aliyuncs.com cannot be used to access resources in qingdaores from the ECS instance. To access resources in qingdaores from the ECS instance, use the public endpoint https://qingdaores.oss-cn-qingdao.aliyuncs.com.

## Use an accelerate endpoint to access OSS

OSS provides transfer acceleration to improve data upload and download speeds. This feature can improve user experiences in uploads and downloads when you transfer data across countries or continents. To use an accelerate endpoint to access a bucket in OSS, you must enable transfer acceleration for the bucket. After you enable transfer acceleration for a bucket, you can access the bucket by using the accelerate endpoint instead of the public endpoint to accelerate data transfer.

Use the global accelerate endpoint as an example. When you use a browser to access a public read object named myphoto.jpg:

-   Use the following URL when you do not need transfer acceleration: https://test.oss-cn-hangzhou.aliyuncs.com/myphoto.jpg
-   Use the following URL when you need transfer acceleration: https://test.oss-accelerate.aliyuncs.com/myphoto.jpg

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## Use endpoints that support IPv6 to access OSS

IPv6 is the latest version of the Internet Protocol developed by the Internet Engineering Task Force \(IETF\) to replace IPv4. IPv6 can provide sufficient IP addresses to meet future requirements. OSS supports access by using dual-stack endpoints that support both IPv4 and IPv6.

You can use a dual-stack endpoint to access your bucket by using both IPv4 and IPv6 from your client. Your DNS server resolves the endpoint and returns the OSS server address to you based on the protocol that you use. For example, the endpoint of the China \(Hangzhou\) region is cn-hangzhou.oss.aliyuncs.com. If you have a bucket named myiotdata in this region, you can access the bucket by using the same endpoint https://myiotdata.cn-hangzhou.oss.aliyuncs.com by using both IPv4 and IPv6 from your client.

The following endpoints support access by using IPv6:

-   China \(Beijing\): https://cn-beijing.oss.aliyuncs.com
-   China \(Shanghai\): https://cn-shanghai.oss.aliyuncs.com
-   China \(Hangzhou\): https://cn-shanghai.oss.aliyuncs.com
-   China \(Shenzhen\): https://cn-shanghai.oss.aliyuncs.com
-   China \(Hong Kong\): https://cn-hongkong.oss.aliyuncs.com
-   China \(Hohhot\): https://cn-huhehaote.oss.aliyuncs.com

