# Rounded rectangle

You can round the corners of a rectangle image stored in OSS by adding rounded-corners parameters. This topic describes the parameters used to round the corners of a rectangle image and provides examples on how to round the corners of a rectangle image.

## Parameters

Operation name: rounded-corners

The following table describes the parameters you can configure.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|r|The radius at which the corners are rounded.|\[1,4096\]|

## Usage notes

-   If the final format of an image is PNG, WebP, or BMP that supports alpha channels, the areas of the image outside the rounded rectangle become transparent. If the final format of the image is JPG, the areas of the image outside the rounded rectangle become white. We recommend that you save the processed image in the PNG format.
-   If the specified radius for the rounded corners is greater than the radius of the largest incircle of the source image, the radius of the largest incircle of the source image is used as the radius to round the corners. In this case, the radius at which the corners are rounded is equal to half of the smallest edge of the source image.

## Examples

An image in the bucket named image-dem in the China \(Hangzhou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Round the corners of the source image.

    Take the following steps to configure parameters:

    -   Set the radius at which the corners are rounded to 30 pixels to perform the rounded-corners operation: `rounded-corners,r_30`.
    -   Save the processed image in the PNG format: `format,jpg`. If the format of the source image is JPG, you can leave this parameter unspecified.
    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rounded-corners,r\_30](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/rounded-corners,r_30)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7856348951/p2502.jpg)

-   Crop the source image before you round the image corners. Save the processed image in the PNG format.

    Take the following steps to configure parameters:

    -   Crop the source image to 100 × 100 pixels from the default start position: `crop,w_100,h_100`.
    -   Set the radius at which the corners are rounded to 10 pixels to perform the rounded-corners operation: `rounded-corners,r_10`.
    -   Save the processed image in the PNG format: `format,png`
    The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w\_100,h\_100/rounded-corners,r\_10/format,png](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w_100,h_100/rounded-corners,r_10/format,png)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7856348951/p2503.png)


