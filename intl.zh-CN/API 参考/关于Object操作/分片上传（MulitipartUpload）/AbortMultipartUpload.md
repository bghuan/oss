# AbortMultipartUpload

AbortMultipartUpload接口用于取消MultipartUpload事件并删除对应的Part数据。

## 注意事项

调用AbortMultipartUpload接口时，有如下注意事项：

-   您需要提供MultipartUpload事件相应的uploadId。
-   取消一个MultipartUpload事件过程中，如果所属的某些Part仍然在上传，那么此次取消操作将无法删除这些Part。
-   取消一个MultipartUpload事件后，您无法再使用此uploadId做任何操作，已经上传的Part数据也会被删除。
-   建议您及时完成分片上传或者取消分片上传，因为已上传但是未完成或未取消的分片会占用存储空间进而产生存储费用。

## 请求语法

```
DELETE /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## 请求元素

|名称|类型|是否必选|描述|
|--|--|----|--|
|uploadId|字符串|是|此次MultipartUpload事件的唯一标识。|

其他公共请求头例如Host、Date等，详情请参见[公共HTTP头定义](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
    Delete /multipart.data?&uploadId=0004B9895DBBB6E****  HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:J/lICfXEvPmmSW86bBAfMmUm****
    ```

-   返回示例

    ```
    HTTP/1.1 204 
    Server: AliyunOSS
    Connection: keep-alive
    x-oss-request-id: 059a22ba-6ba9-daed-5f3a-e48027df****
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/上传文件/分片上传.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/上传文件/分片上传.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/上传文件/分片上传.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/上传文件/分片上传.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/上传文件/分片上传.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/上传文件/分片上传.md)
-   [C](/intl.zh-CN/SDK 示例/C/上传文件/分片上传.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/上传文件/分片上传.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/上传文件/分片上传.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchUpload|404|此uploadId不存在。|

