# Overview

You can add Image Processing \(IMG\) parameters to the GetObject operation to process image objects stored in OSS. For example, you can add image watermarks or convert image formats.

## Parameters

OSS allows you to directly use one or more parameters to process images, or encapsulate multiple IMG parameters in a style to process multiple images at a time. OSS processes the images in the order of the parameters when multiple IMG parameters exist. The following table lists the parameters of IMG.

|IMG|Parameter|Description|
|---|---------|-----------|
|[t1631218.md\#]()|format|Converts images to the format that supports high compression ratios such as HEIF or WebP \(M6\).|
|[Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md)|resize|Resizes images to a specified size.|
|[Incircle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Incircle.md)|circle|Crops images based on the center point of images to ellipses of the specified size.|
|[Custom crop](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Custom crop.md)|crop|Crops rectangular images of the specified size.|
|[Indexed cut](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Indexed cut.md)|indexcrop|Cuts images along the specified horizontal or vertical axis and selects one of the images.|
|[Rounded rectangle](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rounded rectangle.md)|rounded-corners|Crops images to rounded rectangles based on the specified rounded corner size.|
|[Adaptive orientation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Adaptive orientation.md)|auto-orient|Auto-rotates images that include the auto-orient parameter.|
|[Rotate](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Rotate.md)|rotate|Rotates images clockwise based on the specified angle.|
|[Blur](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Blur.md)|blur|Blurs images.|
|[Brightness](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Brightness.md)|bright|Adjusts the brightness of images.|
|[Sharpen](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Sharpen.md)|sharpen|Sharpens images.|
|[Contrast](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Contrast.md)|contrast|Adjusts the contrast of images.|
|[Gradual display](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Gradual display.md)|interlace|Configures gradual display for the JPG images.|
|[Quality transformation](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Quality transformation.md)|quality|Adjusts the quality of images in the JPG and WebP formats.|
|[Format conversion](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Format conversion.md)|format|Converts image formats.|
|[Add watermarks](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Add watermarks.md)|watermark|Adds image or text watermarks to images.|
|[Query average colors](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Retrieve dominant image tones.md)|average-hue|Queries the average tone.|
|[Query information](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Query information.md)|info|Queries image information, including basic information and EXIF information.|

We recommend that you use image style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

## Implementation modes

You can use object URLs, APIs, and SDKs to process images. For more information, see [IMG user guide](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG user guide.md).

## Usage notes

When you use IMG, take note of the following items:

-   Limits on source images
    -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported.
    -   The object size cannot exceed 20 MB.
    -   The width or height of the source image cannot exceed 4,096 pixels when you rotates images.
    -   Each side of the source image cannot exceed 30,000 pixels.
    -   The source image cannot exceed 250 million pixels in total.
-   Limits on thumbnails
    -   The product of dimensions cannot exceed 4,096 × 4,096 pixels.
    -   Each side of the image cannot exceed 4,096 pixels.
-   Limits on image styles

    You can create up to 50 image styles in each bucket. If you require more than 50 styles, contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).


## Release notes

IMG provides two API versions: the later API version and the earlier API version. This topic describes the APIs of the latest version. The APIs of the earlier API version are not updated. For more information about the compatibility between the later and earlier versions of APIs, see [FAQ on using old and new versions of APIs and domain names](/intl.en-US/Developer Guide/Data Processing/Image Processing/FAQ on using old and new versions of APIs and domain names.md).

