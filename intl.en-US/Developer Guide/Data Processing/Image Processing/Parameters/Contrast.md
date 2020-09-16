# Contrast

This topic describes the parameters and examples to adjust the contrast for an image.

## Parameters

Operation name: contrast

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|The contrast of the image. A greater value indicates higher contrast.|\[-100, 100\] A value smaller than 0 indicates contrast lower than the original contrast. A value greater than 0 indicates contrast higher than the original contrast. |

## Examples

-   Adjust the contrast of the source image.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/contrast,-50.`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1956348951/p2532.jpg)

-   Scale down an image to 200 px in width and adjust its contrast.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_200/contrast,-50`

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1956348951/p2534.jpg)


