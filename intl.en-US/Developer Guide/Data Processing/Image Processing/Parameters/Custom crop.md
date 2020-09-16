# Custom crop

This topic describes the parameters and examples to crop an image based on a specified dimension.

## Precautions

Before you crop an image, note the following items:

-   If the specified starting abscissa or ordinate values exceed the source image, the system returns `BadRequest` and the following error message: Advance cut's position is out of image.
-   If the width and height specified from the starting point exceed the source image, the source image is cropped to its bounds.

## Parameters

Operation name: crop

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|w|The width of the cropped area.|\[0, image width\]|
|h|The height of the cropped area.|\[0, image height\]|
|x|The abscissa of the starting point. By default, the origin is located in the upper-left corner.|\[0, image bound\]|
|y|The ordinate of the starting point. By default, the origin is located in the upper-left corner.|\[0, image bound\]|
|g|The location of the origin for cropping. The origin is located in the upper-left corner of any of the nine-cell matrix.|\[nw, north, ne, west, center, east, sw, south, se\]|

The following schematic view shows the available locations for the origin.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2485.png)

## Examples

-   Crop an image from the starting point \(100, 50\) to the bounds.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/crop,x_100,y_50`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2486.jpg)

-   Crop an area of 100 px × 100 px from the starting point \(100, 50\).

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/crop,x_100,y_50,w_100,h_100`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2487.jpg)

-   Crop an area of 200 px × 200 px in the lower-right corner of the source image.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/crop,x_0,y_0,w_200,h_200,g_se`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2488.jpg)

-   Crop an area of 200 px × 200 px in the lower-right corner of an image and stretch the cropped area downward by \(10, 10\).

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/crop,x_10,y_10,w_200,h_200,g_se`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2491.jpg)


