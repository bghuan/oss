# Query information

This topic describes the parameters and examples to query information about an image.

## Precautions

If the source image contains EXIF information and you add the EXIF parameter, EXIF information is returned. Otherwise, only the basic information is returned.

For more information about EXIF, see [EXIF 2.31](http://oss-attachment.cn-hangzhou.oss.aliyun-inc.com/DC-008-Translation-2016-E.pdf).

## Parameters

Operation name: info

The returned image information is in JSON format.

## Examples

-   Query basic information about a source image.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/info`

    View the basic information about the source image in the browser. Example:

    ```
    {
      "FileSize": {"value": "21839"},
      "Format": {"value": "jpg"},
      "ImageHeight": {"value": "267"},
      "ImageWidth": {"value": "400"}
    }
    ```

-   Query the EXIF information of a source image.

    The URL used to process the image is in the following format: `<Source image URL>? x-oss-process=image/info`

    View the EXIF information of the source image in the browser. Example:

    ```
    {
      "Compression": {"value": "6"},
      "DateTime": {"value": "2015:02:11 15:38:27"},
      "ExifTag": {"value": "2212"},
      "FileSize": {"value": "23471"},
      "Format": {"value": "jpg"},
      "GPSLatitude": {"value": "0deg "},
      "GPSLatitudeRef": {"value": "North"},
      "GPSLongitude": {"value": "0deg "},
      "GPSLongitudeRef": {"value": "East"},
      "GPSMapDatum": {"value": "WGS-84"},
      "GPSTag": {"value": "4292"},
      "GPSVersionID": {"value": "2 2 0 0"},
      "ImageHeight": {"value": "333"},
      "ImageWidth": {"value": "424"},
      "JPEGInterchangeFormat": {"value": "4518"},
      "JPEGInterchangeFormatLength": {"value": "3232"},
      "Orientation": {"value": "7"},
      "ResolutionUnit": {"value": "2"},
      "Software": {"value": "Microsoft Windows Photo Viewer 6.1.7600.16385"},
      "XResolution": {"value": "96/1"},
      "YResolution": {"value": "96/1"}}
    ```


