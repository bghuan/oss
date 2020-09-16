# Retrieve dominant image tones

You can retrieve the average tones of images.

## Request operation

Operation name: `average-hue`

## Return format

0xRRGGBB \(RR, GG, and BB are hexadecimal values respectively indicating red, green, and blue\)

## Example

Access the following URL through a browser:

[http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/average-hue)

The following result is returned:

```
{"RGB": "0x5c783b"}
```

Source image:

[http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg)

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5956348951/p2667.jpg)

0x5c783b corresponds to the color RGB \(92,120,59\).

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/5956348951/p2668.png)

