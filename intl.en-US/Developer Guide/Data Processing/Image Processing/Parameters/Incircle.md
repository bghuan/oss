# Incircle

You can use the incircle parameter to process an image stored in OSS as an incircle. This topic describes the parameters and examples to save an image in an ellipse.

## Parameters

Operation name: circle

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|r|Specifies the radius of an incircle.|\[1,4096\]|

## Usage notes

-   If the final format of the image is PNG, WebP, or BMP that supports alpha channels, the areas of the image outside the ellipse become transparent. If the final format of the image is JPG, the areas of the image outside the ellipse become white. We recommend that you save the processed image in PNG.
-   If the value of r is greater than half of the shortest side of the source image, the incircle is returned based on the value of the half of the shortest side of the source image. Value of r = The shortest side of the source image/2.

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Crop an image that has a crop radius of 100 pixels. If the image is saved in the JPG format, the areas of the image outside the ellipse become white.

    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r\_100](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r_100)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5856348951/p2477.jpg)

-   Crop an image that has a crop radius of 100 pixels. If the image is saved in the PNG format, the areas of the image outside the ellipse become transparent.

    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r\_100/format,png](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/circle,r_100/format,png)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5856348951/p2478.png)


