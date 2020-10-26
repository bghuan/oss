# Rotate

You can rotate an image stored in OSS clockwise by adding rotate parameters. This topic describes the parameters used to rotate an image and provides examples on how to rotate an image.

## Parameters

Operation name: rotate

The following table describes the parameters you can configure.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|The degree by which the image is rotated clockwise.|\[0,360\] Default value: 0. A value of 0 indicates that the image is not rotated. |

## Usage notes

-   If an image is not rotated by 90°, 180°, 270°, or 360°, the size of the processed image increases.
-   An image you want to rotate cannot exceeds 4096 × 4096 pixels.

## Examples

An image in the bucket named image-dem in the China \(Hangzhou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Rotate the source image by 90 degrees clockwise.

    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,90](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,90)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8856348951/p2524.jpg)

-   Rotate the source image by 270 degrees clockwise

    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,70](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rotate,70)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8856348951/p2525.jpg)


