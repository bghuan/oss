# Blur

This topic describes the parameters and examples to blur an image.

## Parameters

Operation name: blur

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|r|The blur radius.|\[1,50\] A greater value indicates a blurrier image. |
|s|The standard deviation of a normal distribution.|\[1,50\] A greater value indicates a blurrier image. |

## Examples

-   Set the radius to 3 px and standard deviation to 2 px.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/blur,r_3,s_2`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9856348951/p2526.jpg)

-   Scale down an image to 200 px in width. Blur it by setting the radius to 3 px and standard deviation to 2 px.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_200/blur,r_3,s_2`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9856348951/p2527.jpg)


