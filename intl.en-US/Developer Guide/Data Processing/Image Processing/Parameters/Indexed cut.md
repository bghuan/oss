# Indexed cut

You can use indexed cut parameters to cut a source image stored in OSS based on the specified size and retrieve the required image. This topic describes the parameters and examples to perform indexed cut.

## Parameters

Operation name: indexcrop

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|x|The length of each image partition during horizontal cutting. One of the x and y parameters must be used.|\[1, image width\].|
|y|The length of each image partition during vertical cutting. One of the x and y parameters must be used.|\[1, image height\].|
|i|The image partition selected after cutting.|\[0, maximum number of partitions\).By default, the value is 0, which indicates the first partition. |

## Usage notes

-   If the specified index exceeds that of the cut range, the system returns the source image.
-   If both x and y are specified and their values are valid, the value of y takes effect.

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Cut an image along the horizontal axis

    Requirements and parameters:

    -   Cut the image by 100 pixels along the horizontal axis: `indexcrop,x_100`
    -   Retrieve the first partition: `i_0`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x\_100,i\_0](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,x_100,i_0)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2498.jpg)

-   Cut an image along the vertical axis

    Requirements and parameters:

    -   Cut the image by 100 pixels along the vertical axis: `indexcrop,y_100`
    -   Retrieve the 11th partition: `i_10`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,y\_100,i\_10](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/indexcrop,y_100,i_10)

    The source image is returned because the maximum number of partitions exceeds that of the cut range.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6856348951/p2500.jpg)


