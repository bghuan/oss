# Quality transformation

This topic describes the parameters and examples for quality transformation of an image.

## Precautions

Before you use quality transformation, note the following items:

-   The quality transformation operation compresses images by using image formats. Therefore, this operation supports only images in lossy compression format such as JPG or WebP. You cannot perform this operation on images in lossless compression format such as PNG. Even if you add quality transformation parameters for the PNG images, the image quality remains unchanged after the compression.
-   If you do not specify Q or q, the image may use more storage space. To obtain images of a specific quality value, specify Q.

## Parameters

Operation name: quality

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|q|Determines the relative quality of the image and compresses the source image based on q%. If the source image quality is 100% and q is set to 90, you can obtain an image with a quality value of 90% after you add the `quality,q_90` parameter. If the quality value of the source image is 80% and q is set to 90, you can obtain an image with a quality value of 72% after you add the `quality,q_90` parameter.

 **Note:** The q parameter applies only to JPG images to determine the relative quality of the images. If a source image is in WebP format, this parameter works the same as Q. The absolute quality is specified for the image.

|1 to 100|
|Q|Determines the absolute quality of the image and compresses the source image based on Q%. If the quality value of the source image is smaller than the specified Q value, the source image quality value applies for compression.

 For example, if the quality value of the source image is 95%, you can obtain an image with a quality value of 90% after you add `quality,Q_90`. If the quality value of the source image is 80%, you can obtain an image with a quality value of 80% after you add `quality,Q_90`.

 **Note:** The Q parameter applies only to JPG and WebP images. If you specify both q and Q at the same time, the Q value takes precedence.

|1 to 100|

## Examples

-   Scale down an image to a width of 100 px and height of 100 px. Set the relative quality to 80%.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_100,h_100/quality,q_80`.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2956348951/p2629.jpg)

-   Scale down an image to a width of 100 px and height of 100 px. Set the absolute quality to 80%.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_100,h_100/quality,Q_80`.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2956348951/p2630.jpg)


