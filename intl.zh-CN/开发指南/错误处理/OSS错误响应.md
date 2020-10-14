# OSS错误响应

当用户访问OSS出现错误时，OSS会返回给用户相应的错误码和错误信息，便于用户定位问题，并做出适当的处理。

## 错误响应消息体

当用户访问OSS出错时，OSS会返回给用户一个合适的3xx、4xx或者5xx的HTTP状态码，以及一个application/xml格式的消息体。

错误响应的消息体示例如下：

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
        1D842BC54255****
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

所有错误的消息体中都包括以下几个元素：

-   Code：OSS返回给用户的错误码。
-   Message：OSS给出的详细错误信息。
-   RequestId：用于唯一标识该次请求的UUID。您可以凭借此RequestId请求协助，排查并解决您遇到的问题。
-   HostId：用于标识访问的OSS集群，与用户请求时使用的Host一致。

更多OSS错误码信息，请参见[OSS错误中心](https://error-center.alibabacloud.com/status/product/Oss)。

## HTTP状态码203

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|CallbackFailed|Callback to application server failed, please check your callbackUrl.|因回调参数设置错误或参数格式错误，如ArgumentValue之间的回调参数，不是有效JSON格式等导致上传回调失败。|请参见[上传回调错误及排除]()。|

## HTTP状态码400

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|InvalidBucketName|The specified bucket is not valid.|Bucket命名不符合规范。|了解[Bucket命名规范](/intl.zh-CN/开发指南/基本概念.md)并检查Bucket名称。|
|InvalidObjectName|The Object name can not be empty.|无效的Object名字。|了解[Object命名规范](/intl.zh-CN/开发指南/基本概念.md)并检查Object名称。|
|InvalidObjectName|The Length of Object name must be less than 1024.|
|TooManyBuckets|You have attempted to create more buckets than allowed.|同一阿里云账号在同一地域内创建的Bucket数量不能超过100个。|如需调整Bucket数量上限，请提交工单。|
|RequestIsNotMultiPartContent|Bucket POST must be of the enclosure-type multipart/form-data.|Post操作提交表单编码不是`multipart/form-data`类型。|Post操作提交表单编码必须为`multipart/form-data`，即Header中Content-Type为`multipart/form-data;boundary=xxxxxx`这样的形式，boundary为边界字符串。详情请参见[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)。|
|RequestTimeout|Your socket connection to the server was not read from or written to within the timeout period. Idle connections will be closed.|因网络环境或网络配置等引起的网络超时。|请参见[网络超时处理]()进行排查。|
|NotImplemented|A header you provided implies functionality that is not implemented.|封装API时传递了错误的或者不支持的参数。|请参见[API概览](/intl.zh-CN/API 参考/API概览.md)中相应的API，填写正确的参数格式。|
|InvalidArgument|Invalid Argument Parameter is invalid.|参数格式不符合要求。|
|MaxPOSTPreDataLengthExceededError|Your POST request fields preceding the upload file were too large.|通过Post方法上传的文件大于5 GB。|请参见[PostObject错误及排查]()。|
|FieldItemTooLong|Your name of form field is too long.|Post请求中表单域过大。仅有file表单域大小允许超过4 KB。|
|MalformedPOSTRequest|The body of your POST request is not well-formed multipart/form-data.|表单域格式非法。|
|InvalidPolicyDocument|Invalid policy document, please check.|Post请求中的Policy格式不正确。|
|IncorrectNumberOfFilesInPOSTRequest|POST requires exactly one file upload per request.|Post请求中文件个数非法，Post请求中只能有一个file表单域。|
|EntityTooSmall|Your proposed upload smaller than the minimum allowed size.|采用Post方法上传文件时，通过Post Policy设置表单域的合法值。例如`content-length-range`可以用于指定允许上传Object大小的最大值和最小值。当实体过小或者过大时，则报相应错误。|
|EntityTooLarge|Your proposed upload exceeds the maximum allowed size.|
|EntityTooLarge|Source object Length exceeds the maximum allowed size.|
|MalformedXML|The XML you provided was not well-formed or did not validate against our published schema.|请求中的XML格式非法。|请根据以下具体请求进行排除：-   [DeleteMultipleObjects](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteMultipleObjects.md)
-   [CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)
-   [PutBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/PutBucketLogging.md)
-   [PutBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/PutBucketWebsite.md)
-   [PutBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/PutBucketLifecycle.md)
-   [PutBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/PutBucketReferer.md)
-   [PutBucketCORS](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/PutBucketcors.md) |
|InvalidTargetBucketForLogging|Put bucket log requester is not target bucket owner.|存放Logging的目标Bucket不存在。|请更换有效的目标Bucket。|
|InvalidPart|One or more of the specified parts could not be found or the specified entity tag might not have matched the part's entity tag.|PartNumber或ETag错误导致CompleteMultipartUpload提交的Part无效。|请参见[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)进行排查。|
|FilePartNotExist|The Part you read had been deleted.|CompleteMultipartUpload提交的分片被删除。|
|InvalidPartOrder|The list of parts was not in ascending order.|CompleteMultipartUpload提交的Part未按照PartNumber升序排列。|
|InvalidPartOrder|Part number must be an integer between 1 and 10000, inclusive.|无效的Part顺序。|
|InvalidEncryptionAlgorithmError|The encryption algorithm specified is not valid.|指定x-oss-server-side-encryption的值无效。有效值为AES256或KMS。|请参见[PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)进行排查。|
|InvalidDigest|The Content-MD5 you specified was invalid.|无效的摘要，指定的MD5校验值与文件不符。|
|InvalidTargetType|The symbolic's target file type is invalid.|Object类型为软链接，且软链接指向的目标Object类型仍为软链接。|确保软链接指向的目标Object不能为软链接。|

## HTTP状态码403

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|AccessDenied|AccessDenied|没有相应的访问权限。|请参见[OSS权限问题及排查]()。|
|InvalidAccessKeyId|The OSS Access Key Id you provided is disabled.|AccessKeyId不存在或已失效。|请参见[OSS 403错误及排查]()。|
|InvalidAccessKeyId|The OSS Access Key Id you provided does not exist in our records.|
|InvalidAccessKeyId|The OSS Access Key Id contains non-acceptable characters, which accepts only alphanumeric characters\[0-9a-zA-Z\] and several special characters\[.\_=\]|
|SignatureDoesNotMatch|The request signature we calculated does not match the signature you provided.|签名错误|请参见[签名常见问题](/intl.zh-CN/开发指南/数据安全/签名/签名常见问题.md)进行排查。|
|RequestTimeTooSkewed|The difference between the request time and the current time is too large.|请求发起的时间超过OSS服务器当前时间15分钟。|请参见[OSS 403错误及排查]()。|
|ImageDamage|The image file may be damaged.|图片文件有部分信息丢失或损坏，导致无法正常识别或处理。|
|UserDisable|UserDisable|账号欠费或者由于安全原因，账号被禁用。|请检查是否已经欠费，或联系客服进行安全受限核查。|
|AccessForbidden|CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.|没有配置CORS或CORS配置错误。|请参见[设置跨域访问](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)进行排查。|
|InvalidObjectState|The operation is not valid for the object's state.|下载归档类型Object时，以下两种情况会导致报错无效的Object状态。 -   没有提交RestoreObject请求或者上一次提交RestoreObject已经超时。
-   已经提交RestoreObject请求，但数据的RestoreObject操作还没有完成。

|请参见[RestoreObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/RestoreObject.md)进行排查。|

## HTTP状态码404

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|SymlinkTargetNotExist|The symlink target object does not exist.|不同场景对应的问题原因不同，具体如下：-   客户端显示上传成功，但下载提示404。
    -   Object或Bucket名称拼写错误。
    -   上传成功后未收到OSS返回的requestId，即实际未上传成功。
    -   触发生命周期管理规则，Object被删除。
    -   分片上传或者断点续传时，部分分片上传成功，但最终未完成上传。
-   文件之前一直存在，访问突然提示404。
    -   Object被其他具有合法权限的用户通过OSS控制台、OSS客户端或API等方式删除了。
    -   目标Bucket与其他Bucket存在跨区域复制关系，其他Bucket中执行了删除操作，同步到目标Bucket中，所以文件也被删除了。

|请参考以下方法进行排查：-   确保请求的Bucket名称是正确的。
-   确保请求的Object名称是正确的。如果是软链接，则确保软链接指向的Object是存在的。
-   检查OSS设置的生命周期规则，确认Object未触发删除规则。详情请参见[设置生命周期规则](/intl.zh-CN/控制台用户指南/存储空间管理/基础设置/设置生命周期规则.md)。
-   确认其他具有合法权限的用户未删除请求的Object。
-   检查Bucket未与其他Bucket存在跨区域的复制关系。如果存在该关系，检查源Bucket中是否存在您请求的Object资源。
-   如果是上传Object资源后访问404，确认上传后收到返回的requestId。如果是分片上传或断点续传，以调用CompleteMultipartUpload接口返回的HTTP状态码200以及requestId为准。详情请参见[InitiateMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/InitiateMultipartUpload.md)。 |
|NoSuchBucket|The specified bucket does not exist.|
|NoSuchKey|The specified key does not exist.|
|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|

## HTTP状态码405

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|MethodNotAllowed|The specified method is not allowed against this resource.|使用了OSS不支持的方法来请求访问资源。|请使用[API概览](/intl.zh-CN/API 参考/API概览.md)中支持的请求方式进行重试。|

## HTTP状态码409

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|BucketAlreadyExists|The requested bucket name is not available.|该Bucket已存在或被其他用户占用。|请使用新的Bucket名称创建Bucket。|
|BucketNotEmpty|The bucket you tried to delete is not empty.|要删除的Bucket中有未删除的Object或未完成的分片上传任务。|请参见[DeleteBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/DeleteBucket.md)进行排查。|
|ObjectNotAppendable|The object is not appendable.|不是可追加文件。|OSS有三类文件normal、appendable、multipart，只有appendable类型的文件才能执行AppendObject。|
|PositionNotEqualToLength|Position is not equal to file length.|-   position的值和当前Object的长度不一致。
-   position值为0时，如果没有同名Appendable Object，或者同名Appendable Object长度为0，该请求成功；其他情况均视为position和Object长度不匹配的情形，返回此错误码。

|您可以通过响应头`x-oss-next-append-position`得到下一次position的值，并再次发起请求。由于并发的关系，即使把position的值设为了`x-oss-next-append-position`，该请求依然可能因为`PositionNotEqualToLength`而失败。请参见[AppendObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/AppendObject.md)进行排查。 |
|FileImmutable|The object you specified is immutable.|您试图删除或修改Bucket内处于保护状态的Object。|请确保要删除或修改的Object没有处于受保护状态。请参见[GetBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/GetBucketWorm.md)进行排查。 |

## HTTP状态码411

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|MissingContentLength|You must provide the Content-Length HTTP header.|缺少内容长度，消息为非chunked encoding也没有携带Content-Length。|请确保请求头采用了[chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1)编码，并设置了Content-Length。|

## HTTP状态码412

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|PreconditionFailed|At least one of the pre-conditions you specified did not hold.|预处理错误，下载条件不符合。-   指定了If-Unmodified-Since，但指定的时间早于Object实际修改时间 。
-   指定了If-Match，但源Object的ETag值和您提供的ETag不相等。

|请参见[GetObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObject.md)进行排查。|

## HTTP状态码503

|Code|Message|问题原因|解决方案|
|----|-------|----|----|
|DownloadTrafficRateLimitExceeded|Please reduce your upload request traffic.|下载流量超出限制。|内外网默认下载流量上限为5 Gbit/s。有调整需求请提交工单。|
|UploadTrafficRateLimitExceeded|Please reduce your upload request traffic.|上传流量超出限制。|内外网默认上传流量上限为5 Gbit/s。有调整需求请提交工单。|
|MetaOperationQpsLimitExceeded|Qps limit for the meta operation is exceeded.|超出默认设置的QPS阈值。OSS针对以下管控类API进行QPS限制：

-   Service的操作：GetService \(ListBuckets\)
-   Bucket的操作，如PutBucket、GetBucketLifecycle等
-   跨域资源共享的操作，如PutBucketCORS、GetBucketCORS等
-   LiveChannel的操作，如PutLiveChannel、DeleteLiveChannel等

|建议您延迟几秒后重试。|

## 常见错误及排查

OSS常见错误及排查

-   [上传回调错误及排除]()
-   [403错误及排查]()
-   [PostObject错误及排查]()
-   [权限问题及排查]()
-   [跨域资源共享CORS错误及排除]()
-   [防盗链Referer配置及错误排除]()
-   [STS常见问题及排查]()

SDK、工具类常见错误及排查

-   Java SDK：[常见问题](/intl.zh-CN/SDK 示例/Java/常见问题.md)
-   Node.js SDK：[常见问题](/intl.zh-CN/SDK 示例/Node.js/常见问题.md)
-   [ossutil](https://help.aliyun.com/document_detail/101135.html)
-   [ossfs](/intl.zh-CN/常用工具/ossfs/常见问题.md)
-   [ossftp](/intl.zh-CN/常用工具/ossftp/常见问题.md)

## 附录：支持的操作中添加了不支持的参数

如果在OSS合法的操作中，添加了OSS不支持的参数（例如PUT操作时添加了If-Modified-Since参数），OSS将返回400 Bad Request错误。

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
  <RequestId>57ABD896CCB80C366955****</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

