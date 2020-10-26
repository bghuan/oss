# Query image information

This topic describes the parameters used to query the information about an image and provides examples on how to query the information about an image.

## Usage notes

If the source image contains EXIF information and you add the EXIF parameter, EXIF information is returned. Otherwise, only the basic information is returned.

For more information about EXIF, see [EXIF 2.31](http://oss-attachment.cn-hangzhou.oss.aliyun-inc.com/DC-008-Translation-2016-E.pdf).

## Parameters

Operation name: info

The returned image information is in the JSON format.

## Examples

-   Query the information about an image that does not contain EXIF information

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/info](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/info)

    ```
    {
      "FileSize": {"value": "21839"},
      "Format": {"value": "jpg"},
      "ImageHeight": {"value": "267"},
      "ImageWidth": {"value": "400"}
    }
    ```

-   Query the information about an image that contains EXIF information

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/info](http://image-demo.oss-cn-hangzhou.aliyuncs.com/f.jpg?x-oss-process=image/info)

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


