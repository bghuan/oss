# UploadPart

UploadPart接口用于初始化一个MultipartUpload之后，根据指定的Object名和uploadId来分块（Part）上传数据。

## 注意事项

调用UploadPart接口时，有如下注意事项：

-   调用UploadPart接口上传Part数据前，必须先调用InitiateMultipartUpload接口来获取OSS服务器颁发的uploadId。
-   如果您用同一个partNumber上传了新的数据，那么OSS上已有的partNumber对应的Part数据将被覆盖。
-   OSS会将服务器端收到Part数据的MD5值放在ETag头返回给用户。
-   若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则会对上传的Part进行加密编码，并在UploadPart响应头中返回x-oss-server-side-encryption头，其值表明该Part的服务器端加密算法，详情请参见[InitiateMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/InitiateMultipartUpload.md)。

## 请求语法

```
PUT /ObjectName?partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
```

## 请求元素

|名称|类型|是否必选|描述|
|--|--|----|--|
|partNumber|正整数|是|每一个上传的Part都有一个标识它的号码（partNumber）。取值：1~10000

单个Part的大小限制：100 KB~5 GB。

**说明：** MultipartUpload要求除最后一个Part以外，其他Part的大小都要大于或等于100 KB。因不确定是否为最后一个Part，UploadPart接口并不会立即校验上传Part的大小，只有当CompleteMultipartUpload时才会校验。 |
|uploadId|字符串|是|uploadId用于唯一标识上传的Part属于哪个Object。|

## 示例

请求示例

```
PUT /multipart.data?partNumber=1&uploadId=0004B9895DBBB6EC98E36  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length：6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:J/lICfXEvPmmSW86bBAfMmUm****
[6291456 bytes data]
```

返回示例

```
HTTP/1.1 200 OK
Server: AliyunOSS
Connection: keep-alive
ETag: "7265F4D211B56873A381D321F586****"
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0****
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/上传文件/分片上传.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/上传文件/分片上传.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/上传文件/分片上传.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/上传文件/分片上传.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/上传文件/分片上传.md)
-   [C](/intl.zh-CN/SDK 示例/C/上传文件/分片上传.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/上传文件/分片上传.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/上传文件/分片上传.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/上传文件/分片上传.md)
-   [iOS](/intl.zh-CN/SDK 示例/iOS/上传文件/分片上传.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|InvalidArgument|400|超出partNumber范围（1~10000）|
|InvalidDigest|400|为了保证数据在网络传输过程中不出现错误，用户发送请求时可以携带Content-MD5，OSS计算上传数据的MD5与用户上传的MD5值不一致。|

