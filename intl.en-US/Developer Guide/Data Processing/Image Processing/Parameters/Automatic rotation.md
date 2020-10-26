# Automatic rotation

You can use automatic rotation parameter to specify whether source images stored in OSS are rotated based on the automatic rotation. This topic describes the parameters and examples to rotate images when you configure automatic rotation.

## Parameters

Operation name: auto-orient

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|Specifies whether to perform automatic rotation.|0 and 1-   0: indicates that the orientation of the source image is retained.
-   1: performs automatic rotation on the image. |

## Usage notes

-   If the source image does not have rotation parameters, the operation that you perform to set the auto-orient parameter does not affect the image.
-   Most tools perform automatic rotation on pictures that have rotation parameters. Therefore, the images you view may be automatically rotated.

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg)

-   Resize the image and retain the orientation

    Requirements and parameters:

    -   Resize the image to a width of 100 pixels: `resize,w_100`
    -   Retain the orientation: `auto-orient,0`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w\_100/auto-orient,0](https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w_100/auto-orient,0)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8856348951/p2507.jpg)

-   Resize and automatically rotate the image

    Requirements and parameters:

    -   Resize the image to a width of 100 pixels: `resize,w_100`
    -   Automatically rotate the image: `auto-orient,1`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w\_100/auto-orient,1](https://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/resize,w_100/auto-orient,1)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8856348951/p2508.jpg)


