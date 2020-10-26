# Gradual display

You can use gradual display parameters to configure gradual display for source images stored in OSS. This topic describes the parameters and examples to configure gradual display.

When the network environment is poor or the image size is large, the image can be displayed in two ways on the web page:

-   Standard display: loads and displays images row by row from top to bottom.
-   Gradual display: displays the fuzzy outline of the image, and then loads the image gradually until the complete image is displayed.

The gradual display operation applies only to the source images in the JPG format. If the source image is not in the JPG format, you must add the `format,jpg` parameter to convert the format of the image to JPG.

## Parameters

Operation name: interlace

The following table lists the parameters.

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|\[value\]|Specifies whether to set the image to gradual display.|0 and 1-   1: indicates that the source image is set to gradual display.
-   0: indicates that the source image is set to standard display. |

## Examples

The image-demo bucket that is located in the China \(Hangzhou\) region is used as an example. The public endpoint of the image is `https://image-demo.oss-cn-hangzhou.aliyuncs.com`. The images used are example.jpg and panda.png in the root folder.

-   Resize the image to a width of 200 pixels and set the image to gradual display

    Requirements and parameters:

    -   Resize the image to a width of 200 pixels: `resize,w_200`
    -   Set the image to gradual display: `interlace,1`
    The URL used to process the image is in the following format: [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_200/interlace,1](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_200/interlace,1)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2956348951/p2593.jpg)

-   Save the PNG image as a JPG image and set the image to gradual display

    Requirements and parameters:

    -   Convert the format of the image to JPG: `format,jpg`
    -   Set the image to gradual display: `interlace,1`
    The URL used to process the image is in the following format: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg/interlace,1](https://image-demo.oss-cn-hangzhou.aliyuncs.com/panda.png?x-oss-process=image/format,jpg/interlace,1)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1956348951/p2592.jpg)


