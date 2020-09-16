# Rounded rectangle

This topic describes the parameters and examples to crop an image in rounded rectangle.

## Precautions

-   If the final format of the image is PNG, WebP, or BMP that supports alpha channels, the areas of the image outside the rounded rectangle become transparent. If the final format of the image is JPG, the areas of the image outside the rounded rectangle become white. We recommend that you save the processed image in PNG.
-   If the specified radius is greater than the radius of the largest incircle of the source image, the largest incircle still applies for cropping.

## Parameter

Operation name: rounded-corners

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|r|The radius of the rounded rectangle of an image.|\[1, 4096\] The radius cannot exceed half of the shortest side of the source image. |

## Examples

-   Crop an image by setting the radius of the rounded rectangle to 30 px. Save the cropped image in JPG. The areas of the image outside the rounded rectangle become white.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/rounded-corners,r_30`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7856348951/p2502.jpg)

-   Crop an image to 100 px × 100 px in size. Save the image is PNG by setting the radius of the rounded rectangle to 10 px.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/crop,w_100,h_100/rounded-corners,r_10/format,png`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/7856348951/p2503.png)


