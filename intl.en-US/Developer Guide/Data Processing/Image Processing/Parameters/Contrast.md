# Contrast

Contrast refers to the measurement of different brightness levels between the brightest white and the darkest black of an image, that is, the grayscale contrast of an image. You can use contrast parameter to adjust the contrast of the source images stored in OSS. This topic describes the parameters and examples to adjust the contrast for an image.

## Parameters

Operation name: contrast

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|The contrast of the image.|\[-100,100\] -   A value smaller than 0: reduces the contrast.
-   A value of 0: maintains the contrast.
-   A value greater than 0: increases the contrast. |

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Reduce the contrast by 50

    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,-50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,-50)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1956348951/p2532.jpg)

-   Increase the contrast by 50

    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,50](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/contrast,50)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1956348951/p2534.jpg)


