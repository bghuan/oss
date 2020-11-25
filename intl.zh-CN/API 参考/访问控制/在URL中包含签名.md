# 在URL中包含签名 {#concept_xqh_2df_xdb .concept}

除了使用Authorization Header，用户还可以在URL中加入签名信息，这样用户就可以把该URL转给第三方实现授权访问。

## 示例代码 {#section_wcr_k2f_xdb .section}

URL中添加签名的python示例代码：

```
import base64
import hmac
import sha
import urllib
h = hmac.new("OtxrzxIsfpFjA7SwPzILwy8Bw21TLhquhboDYROV",
             "GET\n\n\n1141889120\n/oss-example/oss-api.pdf",
             sha)
urllib.quote (base64.encodestring(h.digest()).strip())
```

OSS SDK中提供了URL签名方法，详细请参考[SDK文档](../../../../../intl.zh-CN/SDK 参考/SDK 文档简介.md)。

OSS SDK的URL签名实现，请参看下表：

|SDK|URL签名方法|实现文件|
|:--|:------|:---|
|Java SDK|OSSClient.generatePresignedUrl|[OSSClient.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/main/java/com/aliyun/oss/OSSClient.java?spm=a2c4g.11186623.2.6.30uUQV&file=OSSClient.java)|
|Python SDK|Bucket.sign\_url|[api.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/oss2/api.py?spm=a2c4g.11186623.2.7.30uUQV&file=api.py)|
|.Net SDK|OssClient.GeneratePresignedUri|[OssClient.cs](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/sdk/OssClient.cs?spm=a2c4g.11186623.2.8.30uUQV&file=OssClient.cs)|
|PHP SDK|OssClient.signUrl|[OssClient.php](https://github.com/aliyun/aliyun-oss-php-sdk/blob/master/src/OSS/OssClient.php?spm=a2c4g.11186623.2.9.30uUQV)|
|JavaScript SDK|signatureUrl|[Object.js](https://github.com/ali-sdk/ali-oss/blob/master/lib/common/object/signatureUrl.js)|
|C SDK|oss\_gen\_signed\_url|[oss\_object.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk/oss_object.c?spm=a2c4g.11186623.2.11.30uUQV&file=oss_object.c)|

## 实现方式 {#section_rtl_3df_xdb .section}

URL签名示例：

```
http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D
```

URL签名必须至少包含Signature、Expires和OSSAccessKeyId三个参数。

-   `Expires`参数的值是一个[Unix time](https://baike.baidu.com/item/unix时间戳/2078227?fr=aladdin)（自UTC时间1970年1月1号开始的秒数），用于标识该URL的超时时间。如果OSS接收到这个URL请求的时候晚于签名中包含的Expires参数时，则返回请求超时的错误码。例如：当前时间是1141889060，开发者希望创建一个60秒后自动失效的URL，则可以设置Expires时间为1141889120。URL的有效时间默认为3600秒，最大为64800秒。
-   `OSSAccessKeyId` 即密钥中的AccessKeyId。
-   `Signature` 表示签名信息。所有的OSS支持的请求和各种Header参数，在URL中进行签名的算法和[在Header中包含签名](intl.zh-CN/API 参考/访问控制/在Header中包含签名.md#)的算法基本一样。

    ```
    Signature = urlencode(base64(hmac-sha1(AccessKeySecret,
              VERB + "\n" 
              + CONTENT-MD5 + "\n" 
              + CONTENT-TYPE + "\n" 
              + EXPIRES + "\n" 
              + CanonicalizedOSSHeaders
              + CanonicalizedResource)))
    ```

    与Header中包含签名相比主要区别如下：

    -   通过URL包含签名时，之前的Date参数换成Expires参数。
    -   不支持同时在URL和Header中包含签名。
    -   如果多次传入Signature、Expires、OSSAccessKey参数Id，以第一次为准。
    -   先验证请求时间是否晚于Expires时间，然后再验证签名。
    -   将签名字符串放到URL时，注意要对URL进行urlencode。
-   临时用户URL签名时，需要携带`security-token`，格式如下：

    ```
    http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv4%3D&security-token=SecurityToken
    ```


## 细节分析 {#section_cbj_q2f_xdb .section}

-   使用在URL中签名的方式，会将你授权的数据在过期时间内曝露在互联网上，请预先评估使用风险。
-   PUT和GET请求都支持在URL中签名。
-   在URL中添加签名时，Signature，Expires，OSSAccessKeyId顺序可以交换，但是如果Signature，Expires，OSSAccessKeyId缺少其中的一个或者多个，返回403 Forbidden消息。错误码：AccessDenied。
-   如果访问的当前时间晚于请求中设定的Expires时间或时间格式错误，返回403 Forbidden消息。错误码：AccessDenied。
-   如果URL中包含参数Signature，Expires，OSSAccessKeyId中的一个或者多个，并且Header中也包含签名消息，返回400 Bad Request消息 。错误码：InvalidArgument。
-   生成签名字符串时，除Date被替换成Expires参数外，仍然包含content-type、content-md5等上节中定义的Header（请求中虽然仍有Date这个请求Header，但不需要将Date加入签名字符串中）。

