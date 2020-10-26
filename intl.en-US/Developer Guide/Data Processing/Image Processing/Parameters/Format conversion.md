# Format conversion

You can use format conversion parameters to convert the format of a source image stored in OSS. This topic describes the parameters and examples to convert the format of an image.

## Parameters

Operation name: format

The following table lists the parameters.

|Valid value|Description|
|-----------|-----------|
|jpg|Saves the source image in the JPG format. By default, OSS fills the transparent area in white if the source image is in the PNG, WebP, or BMP format that supports alpha channels.|
|png|Saves the source image in the PNG format.|
|webp|Saves the source image in the WebP format.|
|bmp|Saves the source image in the BMP format.|
|gif|Saves the source image in the GIF format. If the source image is not in the GIF format, it is saved in the format of the source image.|
|tiff|Saves the source image in the TIFF format.|

## Usage notes

-   When you perform a standard resize operation on an image, we recommend that you add the format parameter to the end of the last Image Processing \(IMG\) parameter.

    Example: `image/resize,w_100/format,jpg`

-   When you resize and watermark an image, we recommend that you add the format parameter to the end of the resize parameter.

    Example: `image/reisze,w_100/format,jpg/watermark,...`


## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif)

![gif](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5703863061/p139212.png)

-   Convert the format of the source image to PNG

    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,png](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/format,png)

    ![png](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6703863061/p139213.png)

-   Convert the format of the source image to JPG that supports gradual display

    Requirements and parameters:

    -   Set the image to gradual display: `interlace,1`
    -   Convert the format of the image to JPG: `format,jpg`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/interlace,1/format,jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/interlace,1/format,jpg)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3956348951/p2555.jpg)

-   Resize the image to a height of 200 pixels and convert the format of the image to WebP

    Requirements and parameters:

    -   Resize the image to a height of 200 pixels: `resize,w_200`
    -   Convert the format of the image to WebP: `format,webp`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w\_200/format,webp](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.gif?x-oss-process=image/resize,w_200/format,webp)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3956348951/p2559.webp)


