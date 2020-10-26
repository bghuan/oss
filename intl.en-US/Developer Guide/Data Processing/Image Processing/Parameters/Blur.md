# Blur

You can use blur parameters to blur a source image stored in OSS. This topic describes the parameters and examples to blur an image.

## Parameters

Operation name: blur

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|r|The blur radius.|\[1,50\] A greater value indicates a blurrier image. |
|s|The standard deviation of a normal distribution.|\[1,50\] A greater value indicates a blurrier image. |

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

Set the radius to 3 pixels and standard deviation to 2 pixels. The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/blur,r\_3,s\_2](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/blur,r_3,s_2)

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9856348951/p2526.jpg)

