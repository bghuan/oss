# Add watermarks

This topic describes the parameters and examples to add watermarks to an image.

## Precautions

-   You can use watermark images only in the current bucket. Online or local images must be uploaded to the current bucket before these images can be used for watermarking.
-   Only PNG, JPG, and WebP watermark images are supported.
-   Traditional Chinese is not supported for text watermarks.
-   Before you add multiple watermarks to an image, take note of the following items:
    -   You can add up to three watermarks to an image.
    -   A watermark image can be repeatedly used. However, the watermark must be placed at different positions on the source image each time.
    -   Positions of each watermark cannot completely overlap.

## URL-safe Base64 encoding

Many parameters must be encoded in Base64 when you add a watermark. For more information, see [RFC 4648](http://www.ietf.org/rfc/rfc4648.txt?file=rfc4648.txt). Note that the URL-safe Base64 encoding applies only to some specific parameters of watermarking operations, including the text, text color, and font of a text watermark, and the name of an image watermark object. Do not perform URL-safe Base64 encoding on parameters in a signature. Encoding method:

-   Encode the content by using Base64.
-   Replace the plus sign \(+\) in the result with the hyphen \(-\).
-   Replace the forward slash \(/\) in the result with the underscore \(\_\).
-   Retains all equal signs \(=\) in the result.

## Parameters

Operation name: watermark

The following table lists the parameters.

-   Basic parameters

    |Parameter|Description|Value range|
    |---------|-----------|-----------|
    |t|Optional. This parameter specifies the transparency. This parameter makes the added image watermark or text watermark transparent.

|\[0,100\] Default value: 100, indicating no transparency. Unit: % |
    |g|Optional. This parameter specifies the position of a watermark on the target image. The following figure shows the positions of watermarks.

|\[nw,north,ne,west,center,east,sw,south,se\]|
    |x|Optional. This parameter specifies the horizontal margin, which is the horizontal distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, middle left, lower left, upper right, middle right, or lower right of the image.

|\[0,4096\] Default value: 10

Unit: pixel |
    |y|Optional. This parameter specifies the vertical margin, which is the vertical distance between the watermark and the image edge. This parameter takes effect only when the watermark is on the upper left, top center, upper right, lower left, lower center, or lower right of the image.|\[0,4096\] Default value: 10

Unit: pixel |
    |voffset|Optional. This parameter specifies the midline vertical offset. When the watermark is in the middle left, center, or middle right of the image, you can designate the vertical offset of the watermark along the midline.|\[-1000,1000\] Default value: 0

Unit: pixel |

    **Note:**

    -   You can use the horizontal margin, vertical margin, and midline vertical offset to adjust the position of a watermark on an image. You can also use these parameters to adjust the watermark layout when the image has multiple watermarks.
    -   The following figure shows the positions of watermarks based on coordinates.

        ![origin](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4956348951/p2648.png)

-   Image watermark parameters

    |Parameter|Description|Value range|
    |---------|-----------|-----------|
    |image|Required. This parameter specifies the name of the watermark image object. The valid value of the parameter is a Base64-encoded string.

For example, if the name of a watermark image object is the panda.png object in the root directory of a bucket, the string after encoding is `cGFuZGEucG5n`.

**Note:**

    -   You can use images only in the current bucket as watermark images.
    -   The parameter value to encode must be a full object name that contains a prefix.

For example, if the image watermark object is in the panda.png folder of a bucket, the object name to encode is image/panda.png. The object name after encoding is `aW1hZ2UvcGFuZGEucG5n`.

|The Base64-encoded string.|

-   Text watermark parameters

    |Parameter|Description|Value range|
    |---------|-----------|-----------|
    |text|Required. This parameter specifies the content of the text watermark. The valid value of the parameter is a Base64-encoded string.

**Note:**

    -   The content of the text watermark must be a Base64-encoded string by using the following method: encodeText = url\_safe\_base64\_encode\(fontText\).
    -   The content can be a maximum of 64 characters in length.
|The Base64-encoded string.|
    |type|Optional. This parameter specifies the font of the text watermark.

**Note:** The font of the text watermark must be a Base64-encoded string by using the following method: encodeText = url\_safe\_base64\_encode\(fontType\).

|For more information about the Base64-encoded string, see [Literal type encoding table](#table_ipy_1vu_isp). Default value: wqy-zenhei \(value after encoding: d3F5LXplbmhlaQ\) |
    |color|Optional. This parameter specifies the text color of the text watermark.

|The parameter value format must be six hexadecimal numbers. Every two digits define one color component of the RGB color system. For example, 000000 indicates black, and FFFFFF indicates white. Default value: 000000, indicating black |
    |size|Optional. This parameter specifies the size of the text watermark.

|\(0,1000\] Default value: 40

Unit: pixel |
    |shadow|Optional. This parameter specifies the shadow transparency of the text watermark.

|\[0,100\]|
    |rotate|Optional. This parameter specifies the clockwise rotation angle of the text.

|\[0,360\]|
    |fill|Optional. This parameter specifies whether to fill the image with a watermark.

|0 and 1     -   1: The whole image is filled with the watermark.
    -   0: The whole image is not filled with the watermark. |

    The following table describes the valid values of the type parameter and the encoded string of these values.

    |Parameter value|Font name|URL-safe Base64-encoded value|Description|
    |---------------|---------|-----------------------------|-----------|
    |wqy-zenhei|WenQuanYi Zen Hei|d3F5LXplbmhlaQ==|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that d3F5LXplbmhlaQ is also a valid value.|
    |wqy-microhei|WenQuanYi Micro Hei|d3F5LW1pY3JvaGVp|None.|
    |fangzhengshusong|Fangzheng Shusong|ZmFuZ3poZW5nc2h1c29uZw==|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that ZmFuZ3poZW5nc2h1c29uZw is also a valid value.|
    |fangzhengkaiti|Fangzheng Kaiti|ZmFuZ3poZW5na2FpdGk=|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that ZmFuZ3poZW5na2FpdGk is also a valid value.|
    |fangzhengheiti|Fangzheng Heiti|ZmFuZ3poZW5naGVpdGk=|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that ZmFuZ3poZW5naGVpdGk is also a valid value.|
    |fangzhengfangsong|Fangzheng Fangsong|ZmFuZ3poZW5nZmFuZ3Nvbmc=|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that ZmFuZ3poZW5nZmFuZ3Nvbmc is also a valid value.|
    |droidsansfallback|DroidSansFallback|ZHJvaWRzYW5zZmFsbGJhY2s=|The padding character of the equal sign \(=\) can be omitted based on RFC, indicating that ZHJvaWRzYW5zZmFsbGJhY2s is also a valid value.|

-   Text-and-image watermark parameters

    |Parameter|Description|Value range|
    |---------|-----------|-----------|
    |order|Optional. This parameter specifies the order of the text watermark and image watermark.

|0 and 1     -   0: The image watermark is before the text watermark. This is the default value.
    -   1: The text watermark is before the image watermark. |
    |align|Optional. This parameter specifies the alignment of the text watermark and image watermark.

|0, 1, and 2     -   0: The text watermark and the image watermark are aligned based on top alignment. This is the default value.
    -   1: The text watermark and the image watermark are aligned based on center alignment.
    -   2: The text watermark and the image watermark are aligned based on bottom alignment. |
    |interval|Optional. The spacing between the text watermark and image watermark.

|\[0,1000\]|

-   Watermark image preprocessing

    When you preprocess a watermark image, you can use preprocessing operations including: [Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md), [image cropping](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md) \(incircle not supported\), and [image rotation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Adaptive orientation.md). Additionally, another parameter is supported for the resize operation: P. The following table describes the details.

    |Parameter|Description|Value range|
    |---------|-----------|-----------|
    |P|The proportion based on which to scale the watermark image. The value range specifies the percentage of the watermark from the source image. For example, if you set the parameter value to 10 for a source image of 100 pixels x 100 pixels, the size of the watermark image is 10 pixels x 10 pixels. If the source image is 200 pixels x 200 pixels, the size of the watermark image is 20 pixels x 20 pixels.|\[1,100\]|


## Examples

-   Scale the source image to 300 pixels in width and height and 90% in quality, and add the panda.png watermark image to the root folder of the bucket. After URL-safe Base64 encoding, the name of the watermark object is cGFuZGEucG5n. The transparency of the watermark is 90, location is lower right, horizontal margin is 10, and midline vertical offset is 10.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_300,h_300/quality,q_90/watermark,image_cGFuZGEucG5n,t_90,g_se,x_10,y_10`.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4956348951/p2654.jpg)

-   Use panda.png as the watermark image. Scale the watermark image to 30% in width before you add the watermark image. The URL used to preprocess the watermark image is in the following format: `panda.png? x-oss-process=image/resize,P_30`. After URL-safe Base64 encoding, the name of the watermark object is cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA.

    Scale the source image to 400 pixels in width, then use the panda.png image to add watermark for the source image. The transparency of the watermark is 90, location is lower right, horizontal margin is 10, and midline vertical offset is 10.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/resize,w_300/watermark,image_cGFuZGEucG5nP3gtb3NzLXByb2Nlc3M9aW1hZ2UvcmVzaXplLFBfMzA,t_90,g_se,x_10,y_10`.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/4956348951/p2651.jpg)


## FAQ

How do I use online or local images as watermark images?

When OSS IMG is used to add image watermarks to a source image, ensure that the watermark images and source image are from the same bucket. Before you use online or local images to watermark a source image, upload the images to the bucket that contains the source image.

