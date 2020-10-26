# Query the average tone

This topic describes the parameters and examples to query the average tone of an image.

## Parameters

Operation name: average-hue

The returned average tone information is in the following format: 0xRRGGBB. RR, GG, and BB use two hexadecimal digits. RR indicates red. GG indicates green. BB indicates blue.

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5956348951/p2667.jpg)

The URL used to query the average tone of the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue)

The average color information returned in the browser is 0x5c783b. 0x5c783b indicates the RGB color value \(92,120,59\).

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5956348951/p2668.png)

