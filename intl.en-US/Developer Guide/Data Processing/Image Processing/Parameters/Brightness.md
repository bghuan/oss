# Brightness

You can adjust the brightness of an image stored in OSS by adding bright parameters. This topic describes the parameters used to adjust the brightness of an image and provides examples on how to adjust the brightness of an image.

## Parameters

Operation name: bright

The following table describes the parameters you can configure.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|The percentage by which to adjust the image brightness.|\[-100, 100\] -   A value smaller than 0 indicates that the brightness of the image is decreased.
-   A value of 0 indicates that the brightness of the image is not changed.
-   A value greater than 0 indicates that the brightness of the image is increased. |

## Examples

An image in the bucket named image-dem in the China \(Hangzhou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](../images/p139183.png)

-   Increase the brightness of the image by 50 percent

    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,50)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9856348951/p2529.jpg)

-   Decrease the brightness of the image by 50 percent

    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,-50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/bright,-50)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0956348951/p2530.jpg)


