# OSS错误响应

当用户访问OSS出现错误时，OSS会返回给用户相应的错误码和错误信息，便于用户定位问题，并做出适当的处理。

## 错误响应消息体

当用户访问OSS出错时，OSS会返回给用户一个合适的3xx、4xx或者5xx的HTTP状态码，以及一个application/xml格式的消息体。

错误响应的消息体例子：

```
<?xml version="1.0" ?>
<Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Code>
        AccessDenied
    </Code>
    <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
    </Message>
    <RequestId>
        1D842BC5425544BB
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

所有错误的消息体中都包括以下几个元素：

-   Code：OSS返回给用户的错误码。
-   Message：OSS给出的详细错误信息。
-   RequestId：用于唯一标识该次请求的UUID。当你无法解决问题时，可以凭这个RequestId来请求OSS开发工程师的帮助。
-   HostId：用于标识访问的OSS集群，与用户请求时使用的Host一致。

其他特殊的错误信息元素请参照每个请求的具体介绍。

## 错误码汇总

OSS的错误码列表如下：

|HTTP状态码|错误码|描述|原因及解决方案|
|-------|---|--|-------|
|203|CallbackFailed|上传回调失败|因回调参数设置错误或参数格式错误，如ArgumentValue之间的回调参数，不是有效JSON格式等导致上传回调失败。 原因及排查请参考[上传回调错误及排除]()。 |
|400|InvalidBucketName|无效的Bucket名字|Bucket命名不符合规范。 Bucket命名规则请参考[存储空间（Bucket）](/intl.zh-CN/开发指南/基本概念.md)。 |
|InvalidObjectName|无效的Object名字|Object命名不符合规范。 Object命名规则请参考[对象（Object）](/intl.zh-CN/开发指南/基本概念.md)。 |
|TooManyBuckets|Bucket数目超过限制|同一阿里云账号在同一地域内创建的Bucket数量不能超过 30 个。 有调整Bucket数量需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)。 |
|RequestIsNotMultiPartContent|Post请求content-type非法|Post操作提交表单编码不是`multipart/form-data`类型。 Post操作提交表单编码必须为`multipart/form-data`，即Header中Content-Type为multipart/form-data;boundary=xxxxxx 这样的形式，boundary为边界字符串。原因及排查请参考[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)。 |
|RequestTimeout|请求超时|因网络环境或网络配置等引起的网络超时。 请参见[网络超时处理]()进行排查。 |
|NotImplemented|无法处理|封装API时传错参数导致的错误。 请参见[API概览](/intl.zh-CN/API 参考/API概览.md)中相应API规定的参数进行排查。 |
|MaxPOSTPreDataLengthExceededError|Post请求上传文件内容之外的body过大|通过Post方法上传的文件大于5G。 只有`file`的表单域大小允许超过4KB，请参见[PostObject错误及排查]()。 |
|MalformedPOSTRequest|Post请求的body格式非法|表单域格式非法。 排除请参看[PostObject错误及排查]()。 |
|MalformedXML|XML格式非法|请求中的XML格式非法。 请根据以下具体请求进行排除[DeleteObjects](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteMultipleObjects.md)、[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)、[PutBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/PutBucketLogging.md)、[PutBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/PutBucketWebsite.md)、[PutBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/PutBucketLifecycle.md)、[PutBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/PutBucketReferer.md)、[PutBucketCORS](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/PutBucketcors.md)。 |
|InvalidTargetBucketForLogging|Logging操作中有无效的目标bucket|存放Logging的目标bucket无效。请更换有效的目标bucket。|
|InvalidPolicyDocument|无效的Policy文档|Post请求中的Policy格式不正确。 原因及排查请参考[Post Object常见错误]()。 |
|InvalidPart|无效的Part|PartNumber或ETag错误导致CompleteMultipartUpload提交的Part无效。 请参考[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)进行排查。 |
|InvalidPartOrder|无效的part顺序|CompleteMultipartUpload提交的Part顺序无效。 请按照PartNumber升序排列。 请参考[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)进行排查。 |
|InvalidEncryptionAlgorithmError|指定的熵编码加密算法错误|指定x-oss-server-side-encryption的值无效。有效值为AES256或KMS。 请参考[PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)进行排查。 |
|InvalidDigest|无效的摘要|如果用户上传了Content-MD5请求头，OSS会计算body的Content-MD5并检查一致性。如果不一致，将返回InvalidDigest错误码。 请参考[PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)进行排查。 |
|InvalidTargetType|符号链接指向的目标Object类型无效|Object类型为软链接，且软链接指向的目标Object类型仍为软链接。|
|InvalidArgument|参数格式错误|参数格式不符合要求。 请参考[API概览](/intl.zh-CN/API 参考/API概览.md)中相应的API填写正确的参数格式。 |
|IncorrectNumberOfFilesInPOSTRequest|Post请求中文件个数非法|Post请求表单域中只能有一个file域。 请参考[PostObject错误及排查]()。 |
|FilePartNotExist|文件Part不存在|CompleteMultipartUpload提交的分片没有上传。 请参考[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)进行排查。 |
|FieldItemTooLong|Post请求中表单域过大|只有file表单域大小允许超过4KB。 请参考[PostObject错误及排查]()。 |
|EntityTooSmall|实体过小|采用Post方法上传文件时，通过Post Policy设置表单域的合法值。例如content-length-range可以用于指定上传Object的最小和最大允许大小。 该condition支持contion-length-range匹配方式。 当过小或者过大时，则报相应错误。 请参考[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)进行排查。 |
|EntityTooLarge|实体过大|
|403|AccessDenied|拒绝访问|无指定操作的权限。 请参考[OSS权限问题及排查]()。 |
|InvalidAccessKeyId|无效的AccessKeyId|AccessKeyId无效或过期。 请参考[OSS 403错误及排查]()。 |
|InvalidObjectState|无效的Object状态|下载归档类型Object时，以下两种情况会导致报错无效的Object状态。 -   没有提交RestoreObject请求或者上一次提交RestoreObject已经超时。
-   已经提交RestoreObject请求，但数据的RestoreObject操作还没有完成。

 请参考[RestoreObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/RestoreObject.md)进行排查。 |
|RequestTimeTooSkewed|发送请求的时间与OSS收到请求的时间间隔超出了15分钟|请检查发送请求设备的系统时间，并根据时区调整到正确时间。 请参考[OSS 403错误及排查]()。 |
|SignatureDoesNotMatch|签名错误|请参见[签名常见问题](/intl.zh-CN/开发指南/数据安全/签名/签名常见问题.md)进行排查。

 请参见签名常见问题进行排查。 |
|404|SymlinkTargetNotExist|软链接指向的目标Object不存在|Object类型为软链接，且软链接指向的目标Object不存在。|
|NoSuchBucket|Bucket不存在|当请求一个不存在的bucket时，会报此类错误。|
|NoSuchKey|Object不存在|当请求一个不存在的object时，会报此类错误。|
|NoSuchUpload|MultipartUpload ID不存在|没有初始化分片上传或者初始化的分片上传过期。 请参见[InitiateMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/InitiateMultipartUpload.md)进行排查。 |
|405|MethodNotAllowed|不支持的方法|当操作行为不支持的话，会报此类错误。|
|409|BucketAlreadyExists|Bucket已存在|该Bucket已存在。请使用新的Bucket名称创建Bucket。 Bucket命名规则请参考[存储空间（Bucket）](/intl.zh-CN/开发指南/基本概念.md)。 |
|BucketNotEmpty|Bucket不为空|要删除的Bucket中有未删除的Object或未完成的分片上传任务。 请参见[DeleteBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/DeleteBucket.md)进行排查。 |
|ObjectNotAppendable|不是可追加文件|OSS有三种文件类型，分别为normal、appendable、multipart。只有appendable类型的文件才能执行AppendObject操作。|
|PositionNotEqualToLength|Append的位置和文件长度不相等|-   position的值和当前Object的长度不一致。

**说明：** 您可以通过响应头x-oss-next-append-position得到下一次position，并再次进行请求。由于并发的关系，即使把position的值设为了x-oss-next-append-position，该请求依然可能因为PositionNotEqualToLength而失败。

-   position值为0时，如果没有同名Appendable Object，或者同名Appendable Object长度为0，该请求成功；其他情况均视为position和Object长度不匹配的情形，返回此错误码。 |
|411|MissingContentLength|缺少内容长度|请求头没有采用[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码方式，且没有Content-Length参数。|
|412|PreconditionFailed|预处理错误|-   指定了If-Unmodified-Since，但指定的时间早于Object实际修改时间 。
-   指定了If-Match，但源Object的ETag值和您提供的ETag不相等。

 请参见[GetObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObject.md)进行排查。 |
|503|DownloadTrafficRateLimitExceeded|下载流量超过限制|内外网默认下载流量上限为5Gbit/s。有调整需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)。|
|UploadTrafficRateLimitExceeded|上传流量超过限制|内外网默认上传流量上限为5Gbit/s。有调整需求请[提交工单](https://workorder.console.aliyun.com/#/ticket/createIndex)。|
|MetaOperationQpsLimitExceeded|超出默认设置的QPS阈值|OSS针对以下管控类API进行QPS限制： -   Service的操作：GetService \(ListBuckets\)
-   Bucket的操作，如PutBucket、GetBucketLifecycle等
-   跨域资源共享的操作，如PutBucketCORS、GetBucketCORS等
-   LiveChannel的操作，如PutLiveChannel、DeleteLiveChannel等

 超出阈值，则返回503。建议您延迟几秒后进行重试。 |

## 常见错误及排查

OSS常见错误及排查

-   [上传回调错误及排除]()
-   [403错误及排查]()
-   [PostObject错误及排查]()
-   [权限问题及排查]()
-   [跨域资源共享CORS错误及排除]()
-   [防盗链Referer配置及错误排除]()
-   [STS常见问题及排查]()

SDK/Tool常见错误及排查

-   Java SDK：[常见问题](/intl.zh-CN/SDK 示例/Java/常见问题.md)
-   Python SDK：[常见问题](/intl.zh-CN/SDK 示例/Python/常见问题.md)
-   C SDK：[常见问题](/intl.zh-CN/SDK 示例/C/常见问题.md)
-   Node.js SDK：[常见问题](/intl.zh-CN/SDK 示例/Node.js/常见问题.md)
-   [ossfs](/intl.zh-CN/常用工具/ossfs/常见问题.md)
-   [ossftp](/intl.zh-CN/常用工具/ossftp/常见问题.md)
-   [ossutil](https://help.aliyun.com/document_detail/101135.html)

## 不支持的操作

如果试图以OSS不支持的操作来访问某个资源，返回405 Method Not Allowed错误。

错误请求示例：

```
ABC /1.txt HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Date: Thu, 11 Aug 2016 03:53:40 GMT
Authorization: signatureValue
```

返回示例：

```
HTTP/1.1 405 Method Not Allowed
Server: AliyunOSS
Date: Thu, 11 Aug 2016 03:53:44 GMT
Content-Type: application/xml
Content-Length: 338
Connection: keep-alive
x-oss-request-id: 57ABF6C8BC4D25D86CBA5ADE
Allow: GET DELETE HEAD PUT POST OPTIONS
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>MethodNotAllowed</Code>
  <Message>The specified method is not allowed against this resource.</Message>
  <RequestId>57ABF6C8BC4D25D86CBA5ADE</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Method>abc</Method>
  <ResourceType>Bucket</ResourceType>
</Error>
```

**说明：** 如果访问的资源是/bucket/， 则ResourceType是bucket。如果访问的资源是/bucket/object，则ResourceType是object。

## 支持的操作中添加了不支持的参数

如果在OSS合法的操作中，添加了OSS不支持的参数（例如在PUT的时候，加入If-Modified-Since参数），OSS会返回400 Bad Request错误。

错误请求示例：

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2016 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2016 01:43:51 GMT
Content-Length: 363
```

返回示例：

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2016 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955187E
x-oss-server-time: 0
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented.</Message>
  <RequestId>57ABD896CCB80C366955187E</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

