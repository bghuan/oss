# Quality transformation

The quality transformation operation uses the format of the source image to compress the image. You can use the quality transformation parameters to modify the quality of source images stored in OSS. This topic describes the parameters and examples for quality transformation of an image.

Quality transformation applies only to JPG and WebP images.

## Parameters

Operation name: quality

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|q|Specifies the relative quality of the image and compresses the source image based on percentage.If the source image quality is 100%, you can obtain an image that has a quality value of 90% after you add the `quality,q_90` parameter. If the quality value of the source image is 80%, you can obtain an image that has a quality value of 72% after you add the `quality,q_90` parameter.

**Note:** The q parameter applies only to JPG images to specify the relative quality of the images. If a source image is in the WebP format, this parameter works the same as Q. The absolute quality is specified for the image.

|\[1,100\]|
|Q|Specifies the absolute quality of the image and compresses the source image based on Q%. If the quality value of the source image is smaller than the specified Q value, the source image quality value applies to compression.For example, if the quality value of the source image is 95%, you can obtain an image that has a quality value of 90% after you add `quality,Q_90`. If the quality value of the source image is 80%, you can obtain an image that has a quality value of 80% after you add `quality,Q_90`.

**Note:** The Q parameter applies only to JPG and WebP images. If you specify both q and Q at the same time, the Q value takes precedence.

|\[1,100\]|

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. Public endpoint of the image:

[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![Source image](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8812863061/p139183.png)

-   Transform the relative quality of an image

    Requirements and parameters:

    -   Resize the image to a width of 100 pixels: `resize,w_100`
    -   Set the quality value of the image to 80%: `quality,q_80`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_100/quality,q\_80](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/quality,q_80)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2956348951/p2629.jpg)

-   Transform the absolute quality of the image

    Requirements and parameters:

    -   Resize the image to a width of 100 pixels: `resize,w_100`
    -   Set the absolute value of the image to 80%: `quality,Q_80`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_100/quality,Q\_80](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/quality,Q_80)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2956348951/p2630.jpg)


