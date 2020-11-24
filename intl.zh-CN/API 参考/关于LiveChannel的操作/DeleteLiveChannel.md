# DeleteLiveChannel

DeleteLiveChannel接口用于删除指定的LiveChannel。

## 请求语法

**说明：**

-   当有客户端正在向LiveChannel推流时，删除请求会失败。
-   本接口只会删除LiveChannel本身，不会删除推流生成的文件。

```
DELETE /ChannelName?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## 示例

请求示例

```
DELETE /test-channel?live HTTP/1.1
Date: Thu, 25 Aug 2016 07:32:26 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:ZbfvQ3XwmYEE8O9CX8kwVQY****
```

返回示例

```
HTTP/1.1 204
content-length: 0
server: AliyunOSS
connection: close
x-oss-request-id: 57BE9F0AB92475920B0*****
date: Thu, 25 Aug 2016 07:32:26 GMT
```

## SDK

[Java]()

