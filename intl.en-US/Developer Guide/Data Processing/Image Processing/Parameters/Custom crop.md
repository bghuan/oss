# Custom crop

You can use custom crop parameters to crop a rectangular image based on a specified size from a source image stored in OSS. This topic describes the parameters and examples to crop an image based on a specified dimension.

## Parameters

Operation name: crop

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|w|The width of the cropped area.|\[0, image width\].Default value: the maximum value. |
|h|The height of the cropped area.|\[0, image height\].Default value: the maximum value. |
|x|The abscissa of the starting point. By default, the origin is located in the upper-left corner.|\[0, image bound\]|
|y|The ordinate of the starting point. By default, the origin is located in the upper-left corner.|\[0, image bound\]|
|g|The location of the origin for cropping. The origin is located in the upper-left corner of any of the nine-cell matrix.|-   nw
-   north
-   ne
-   west
-   center
-   east
-   sw
-   south
-   se

The following schematic view shows the available locations for the origin.|

The following schematic view shows the available locations for the origin.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2485.png)

## Usage notes

Before you crop an image, take note of the following items:

-   If the specified starting abscissa or ordinate values exceed those of the source image, the system returns `BadRequest` and the following error message: Advance cut's position is out of image.
-   If the width and height specified from the starting point exceed those of the source image, the source image is cropped to its boundaries.

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Crop an image from the starting point \(100, 50\) to the bounds

    Requirements and parameters:

    -   From the starting point \(100, 50\): `crop,x_100,y_50`
    -   To the bounds: By default, the maximum values of w and h are used for cropping. Therefore, the w and h parameters can be omitted.
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_100,y\_50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_100,y_50)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2486.jpg)

-   Crop an area of 100 × 100 pixels from the starting point \(100, 50\)

    Requirements and parameters:

    -   From the starting point \(100, 50\): `crop,x_100,y_50`
    -   An area of 100 × 100 pixels: `w_100,h_100`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_100,y\_50,w\_100,h\_100](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_100,y_50,w_100,h_100)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2487.jpg)

-   Crop an area of 200 × 200 pixels in the lower-right corner of the source image

    Requirements and parameters:

    -   From the starting point in the lower-right corner of the source image: `crop,g_se`
    -   An area of 200 × 200 pixels: `w_200,h_200`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w\_200,h\_200,g\_se](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,w_200,h_200,g_se)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2488.jpg)

-   Crop an area of 200 × 200 pixels in the lower-right corner of an image and stretch the cropped area downward by \(10, 10\)

    Requirements and parameters:

    -   From the starting point in the lower-right corner of an image and stretch the cropped area downward by \(10, 10\): `crop,g_se,x_10,y_10`
    -   An area of 200 × 200 pixels: `w_200,h_200`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x\_10,y\_10,w\_200,h\_200,g\_se](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/crop,x_10,y_10,w_200,h_200,g_se)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2491.jpg)


