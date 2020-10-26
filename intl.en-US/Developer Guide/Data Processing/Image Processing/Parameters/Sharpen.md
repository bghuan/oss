# Sharpen

You can sharpen an image stored in OSS by adding sharpen parameters. This topic describes the parameters used to sharpen an image and provides examples on how to sharpen an image.

## Parameters

Operation name: sharpen

The following table describes the parameters you can configure.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|The degree of sharpness.|\[50,399\] A greater value indicates a clearer image. However, an overlarge value may result in image artifacts. We recommend that you set this parameter to 100 for optimal effects. |

## Examples

An image in the bucket named image-dem in the China \(Hangzhou\) region is used in this example. The following URL is used to access the image over the Internet:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image ](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

Sharpen the source image. The degree of sharpness is set to 100. The following URL is used to process the image: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/sharpen,100](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/sharpen,100)

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0956348951/p2537.jpg)

