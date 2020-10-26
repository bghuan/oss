# Enable transfer acceleration

OSS provides transfer acceleration to help your enterprise reach its full potential across all Alibaba Cloud regions. OSS uses data centers distributed around the globe to implement transfer acceleration. When a data transfer request is sent, the request is resolved and routed over the optimal network path and protocol to the data center where your bucket is located. Transfer acceleration allows customers to access OSS more quickly to improve customer experience and make business closer to customers.

Real-name registration is complete.

You can complete real-name registration by submitting your relevant information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page.

## Usage notes

When you use transfer acceleration, take note of the following items:

-   When you enable or disable transfer acceleration, the configurations take effect in 30 minutes.
-   Access is accelerated if you use the accelerate endpoint to access a bucket that has transfer acceleration enabled.
-   When the endpoint of your tool is set to an accelerate endpoint, you can perform operations only on the bucket that has transfer acceleration enabled.
-   When transfer acceleration is enabled, other endpoints function normally. You can switch between endpoints at any time.
-   Transfer acceleration applies only to acceleration of access over the Internet.
-   For data transfer security reasons, HTTP used in the actual transfer endpoint is replaced with HTTPS when the peer end receives the request. Therefore, if you access OSS over HTTPS, the actual accelerate endpoint-based URL uses HTTPS. If you access OSS over HTTP, HTTP used in the actual accelerate endpoint-based URL may be changed to HTTPS.

For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Transmission** \> **Transfer Acceleration**.

4.  Click **Configure** to turn on or turn off Transfer Acceleration.

5.  Click **Save**.

    Alternatively, on the **Overview** tab, find the **Domain Names** section. Click **Enable** in the Bucket Domain Name column corresponding to **Transfer-Acceleration-based Access**. In the Transfer Acceleration dialog box, turn on **Transfer Acceleration**. Click **Save**.

    After transfer acceleration is enabled, two public endpoints are generated for the bucket.

    -   Global accelerate endpoint: The format is BucketName.oss-accelerate.aliyuncs.com. Transfer acceleration access points are distributed across the world. You can use this endpoint to accelerate access to buckets all over the world.
    -   Accelerate endpoint outside mainland China: The format is BucketName.oss-accelerate-overseas.aliyuncs.com. Transfer acceleration access points are distributed across the world except mainland China. We recommend that you use accelerate endpoints outside mainland China only when you need to bind a custom domain name for which you have not applied for an ICP filing to a bucket outside mainland China, and use the global accelerate endpoint in other scenarios. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).
    **Note:**

    -   You can compare the speeds of access from different regions when the accelerate and regular endpoints are used. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).

    -   Additional fees are charged when you use transfer acceleration. For more information, see [Object Storage Service Pricing](https://cn.aliyun.com/price/product#/oss/detail).

