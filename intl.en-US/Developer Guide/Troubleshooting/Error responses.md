# Error responses

If an error occurs when you access OSS, OSS returns the error code and error message so that you can identify and rectify the error.

## Response message body

If an error occurs when you access OSS, OSS returns HTTP status code 3xx, 4xx, or 5xx and a message body in the application/xml format.

Example of the message body of an error response:

```
<? xml version="1.0" ? >
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

The message body of an an error response includes the following elements:

-   Code: the error code that OSS returns to the user.
-   Message: the detailed error message provided by OSS.
-   RequestId: the UUID that uniquely identifies a request. If you need help from OSS development engineers to handle an exception, provide them with the RequestId.
-   HostId: the ID of the host in the accessed OSS cluster, which is the same as the host ID specified in the request.

For more information about OSS error codes, see [OSS error center](https://error-center.alibabacloud.com/status/product/Oss).

## HTTP status code 203

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|CallbackFailed|Callback to application server failed, please check your callbackUrl.|The error message returned because callback parameters are incorrectly configured or in invalid formats. For example, upload callback fails because the callback parameters within the ArgumentValue is not in the valid JSON format.|See [Upload callback errors and troubleshooting]() to find detailed solutions.|

## HTTP status code 400

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|InvalidBucketName|The specified bucket is not valid.|The error message returned because the bucket name is invalid.|See [Bucket](/intl.en-US/Developer Guide/Terms.md) to learn the bucket naming conventions and then check the bucket name.|
|InvalidObjectName|The Object name can not be empty.|The error message returned because the object name is invalid.|See [Object](/intl.en-US/Developer Guide/Terms.md) to learn the object naming conventions and then check the object name.|
|InvalidObjectName|The Length of Object name must be less than 1024.|
|TooManyBuckets|You have attempted to create more buckets than allowed.|An Alibaba Cloud account can create up to 100 buckets in a region.|To adjust the limit, submit a ticket.|
|RequestIsNotMultiPartContent|Bucket POST must be of the enclosure-type multipart/form-data.|The error message returned because the Content-Type header in the PostObject request is not `multipart/form-data`.|The form submitted by the PostObject operation must be encoded in the `multipart/form-data` format. For example, the Content-Type header must be in the `multipart/form-data;boundary=xxxxxx` format, where boundary is a boundary string. For more information, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).|
|RequestTimeout|Your socket connection to the server was not read from or written to within the timeout period. Idle connections will be closed.|The error message returned because the request times out due to network environment or configurations.|See [Network connection timeout handling]() to handle problems.|
|NotImplemented|A header you provided implies functionality that is not implemented.|The error message returned because invalid parameters are passed when the API is encapsulated.|See [API overview](/intl.en-US/API Reference/API overview.md) and configure valid parameters.|
|InvalidArgument|Invalid Argument Parameter is invalid.|The error message returned because the parameter format is invalid.|
|MaxPOSTPreDataLengthExceededError|Your POST request fields preceding the upload file were too large.|The error message returned because size of the object uploaded by PostObject is larger than 5 GB.|See [PostObject errors and troubleshooting]() to handle the problem.|
|FieldItemTooLong|Your name of form field is too long.|The error message returned because the total length of the form fields in the POST request exceeds the limit. Only the file form field can be larger than 4 KB in size.|
|MalformedPOSTRequest|The body of your POST request is not well-formed multipart/form-data.|The error message returned because the format of the form field is invalid.|
|InvalidPolicyDocument|Invalid policy document, please check.|The error message returned because the policy format in the PostObject request is invalid.|
|IncorrectNumberOfFilesInPOSTRequest|POST requires exactly one file upload per request.|The error message returned because the number of objects in the PostObject request is invalid. A PostObject request can contain only one file form field.|
|EntityTooSmall|Your proposed upload smaller than the minimum allowed size.|Post policy is configured to specify the valid values of form fields when PostObject is called to upload files. For example, `content-length-range` can be used as a condition to set the maximum and minimum size of an uploaded object. Corresponding error messages are returned when the value exceeds the range.|
|EntityTooLarge|Your proposed upload exceeds the maximum allowed size.|
|EntityTooLarge|Source object Length exceeds the maximum allowed size.|
|MalformedXML|The XML you provided was not well-formed or did not validate against our published schema.|The error message returned because the XML format in the request is invalid.|See the following topics to handle the problems based on requests:-   [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md)
-   [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md)
-   [PutBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/PutBucketLogging.md)
-   [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md)
-   [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md)
-   [PutBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/PutBucketReferer.md)
-   [PutBucketCORS](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketcors.md) |
|InvalidTargetBucketForLogging|Put bucket log requester is not target bucket owner.|The error message returned because the destination bucket specified to store logs does not exist.|Specify a valid destination bucket.|
|InvalidPart|One or more of the specified parts could not be found or the specified entity tag might not have matched the part's entity tag.|The error message returned because a part uploaded by CompleteMultipartUpload is invalid due to invalid PartNumber or ETag.|For more information about troubleshooting, see [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).|
|FilePartNotExist|The Part you read had been deleted.|The error message returned because a part submitted by CompleteMultipartUpload is deleted.|
|InvalidPartOrder|The list of parts was not in ascending order.|The error message returned because the parts submitted by CompleteMultipartUpload are not in ascending order of PartNumber.|
|InvalidPartOrder|Part number must be an integer between 1 and 10000, inclusive.|The error message returned because the part sequence is invalid.|
|InvalidEncryptionAlgorithmError|The encryption algorithm specified is not valid.|The error message returned because the specified value of x-oss-server-side-encryption is invalid. Valid values: AES256 and KMS.|See [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md) to handle the problem.|
|InvalidDigest|The Content-MD5 you specified was invalid.|The error message returned because the digest is invalid, and the specified MD5 checksum value does not match the MD5 hash of the object.|
|InvalidTargetType|The symbolic's target file type is invalid.|The error message returned because the object is a symbolic link and the object that the link points to is also a symbolic link.|Ensure that the symbolic link does not point to another symbolic link.|

## HTTP status code 403

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|AccessDenied|AccessDenied|The error message returned because you do not have access permissions.|For more information, see [OSS permission]().|
|InvalidAccessKeyId|The OSS Access Key Id you provided is disabled.|The error message returned because the AccessKey ID does not exist or is invalid.|See [OSS 403]() to handle the problem.|
|InvalidAccessKeyId|The OSS Access Key Id you provided does not exist in our records.|
|InvalidAccessKeyId|The OSS Access Key Id contains non-acceptable characters, which accepts only alphanumeric characters\[0-9a-zA-Z\] and several special characters\[. \_=\]|
|SignatureDoesNotMatch|The request signature we calculated does not match the signature you provided.|The error message returned because a signature error occurs.|See [FAQ](/intl.en-US/Developer Guide/Data security/Signature/FAQ.md) to handle the problem.|
|RequestTimeTooSkewed|The difference between the request time and the current time is too large.|The error message returned because the time when the request is initiated is 15 minutes before the current time of the OSS server.|See [OSS 403]() to handle the problem.|
|ImageDamage|The image file may be damaged.|The error returned because the image is partially lost or damaged so that the image cannot be recognized or processed.|
|UserDisable|UserDisable|The error message returned because the account is deactivated due to overdue payment or security reasons.|Check whether you have overdue payment or contact technical support to perform a security check.|
|AccessForbidden|CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.|The error message returned because CORS is not configured or the CORS configuration is incorrect.|See [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md) to handle the problem.|
|InvalidObjectState|The operation is not valid for the object's state.|When you download an Archive object, the state of the object is invalid in the following two scenarios: -   The RestoreObject request for the object is not initiated or times out.
-   The RestoreObject request for the object is initiated but the object is not restored.

|See [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) to handle the problem.|

## HTTP status code 404

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|SymlinkTargetNotExist|The symlink target object does not exist.|The error message returned because of different reasons in different scenarios.-   The client shows that the object is uploaded. But the 404 code is returned when you download the object.
    -   The object or the bucket name is incorrect.
    -   The request ID is not returned to the client, that is, the object is not successfully uploaded.
    -   The object is deleted based on lifecycle rules.
    -   Only some parts but not the entire object is uploaded in multipart upload or resumable upload.
-   The 404 code is returned when you access an existing object.
    -   The object is deleted by an authorized user by using the OSS console, OSS client, or API operations.
    -   Cross-region replication \(CRR\) is configured for the bucket. The delete operation performed on the object that has the same name in another bucket is synchronized to the bucket so that the object is deleted.

|Use the following methods to identify the problem:-   Ensure that the bucket name in the request is correct.
-   Ensure that the object name in the request is correct. Ensure that symbolic link objects point to existing objects.
-   Check the lifecycle rules configured for the bucket to ensure that the object is not deleted based on the rules. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   Ensure that the requested object is not deleted by other authorized users.
-   Check whether CRR is configured between the bucket and other buckets. If CRR is configured for the bucket, check whether the requested object is stored in the source bucket.
-   If the 404 code is returned when you access an object that you upload, ensure that the RequestId is returned after the object is uploaded. In multipart upload or resumable upload, ensure that HTTP status code 200 and the RequestId is returned for the CompleteMultipartUpload operation. For more information, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md). |
|NoSuchBucket|The specified bucket does not exist.|
|NoSuchKey|The specified key does not exist.|
|NoSuchUpload|The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.|

## HTTP status code 405

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|MethodNotAllowed|The specified method is not allowed against this resource.|The error message returned because the method used to access OSS resources is not supported.|Use the methods described in [API overview](/intl.en-US/API Reference/API overview.md) to access OSS resources.|

## HTTP status code 409

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|BucketAlreadyExists|The requested bucket name is not available.|The error message returned because the specified bucket already exists or is owned by another user.|Specify another name for the bucket that you want to create.|
|BucketNotEmpty|The bucket you tried to delete is not empty.|The error message returned because the bucket to delete contains objects that are not deleted or incomplete multipart upload tasks.|See [DeleteBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/DeleteBucket.md) to handle the problem.|
|ObjectNotAppendable|The object is not appendable.|The error message returned because the object is not an append object.|OSS objects can be categorized into three types based on the upload method: normal objects, append objects, and multipart objects. The AppendObject operation can be performed only on append objects.|
|PositionNotEqualToLength|Position is not equal to file length.|-   The value of position does not match the current object length.
-   The request is successful when the value of position is 0 and the length of an append object with the same name is 0 or the append object with the same name does not exist. In other cases, this error code is returned because the position and the length of the object do not match.

|You can obtain the position for the next operation from the response header `x-oss-next-append-position`, and then send the next request. Multiple requests may be sent concurrently, even if you set the value of `x-oss-next-append-position` in one request, the request may still fail because the value is not updated immediately.``See [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md) to handle the problem. |
|FileImmutable|The object you specified is immutable.|The error message returned because the object that you want to delete or modify is protected.|Ensure that the object to delete or modify is not protected.See [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md) to handle the problem. |

## HTTP status code 411

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|MissingContentLength|You must provide the Content-Length HTTP header.|The error message returned because the request does not use chunked encoding and does not contain the Content-Length header.|Ensure that the request use [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) and contain the Content-Length header.|

## HTTP status code 412

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|PreconditionFailed|At least one of the pre-conditions you specified did not hold.|The error message returned because the object to download does not meet the specified conditions.-   The If-Unmodified-Since header is specified in the request, but the specified time is earlier than the modified time of the requested object.
-   The If-Match header is specified in the request, but the ETag value provided in the request is different from the ETag value of the requested object.

|See [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md) to handle the problem.|

## HTTP status code 503

|Code|Message|Cause|Solution|
|----|-------|-----|--------|
|DownloadTrafficRateLimitExceeded|Please reduce your upload request traffic.|The error message returned because the downloading traffic exceeds the limit.|The default limit of the downloading traffic over the Internet and internal networks is 5 Gbit/s. To adjust the limit, submit a ticket.|
|UploadTrafficRateLimitExceeded|Please reduce your upload request traffic.|The error message returned because the uploading traffic exceeds the limit.|The default limit of the uploading traffic through the Internet and internal networks is 5 Gbit/s. To adjust the limit, submit a ticket.|
|MetaOperationQpsLimitExceeded|Qps limit for the meta operation is exceeded.|The error message returned because the QPS exceeds the default limit.OSS limits the QPS for the following API operations:

-   Operations related to services, such as GetService \(ListBuckets\)
-   Operations related to buckets, such as PutBucket and GetBucketLifecycle
-   Operations related to CORS, such as PutBucketCORS and GetBucketCORS
-   Operations related to LiveChannel, such as PutLiveChannel and DeleteLiveChannel.

|We recommend that you perform the operation again after a few seconds.|

## Common errors and troubleshooting

For more information about the common errors of OSS and their troubleshooting methods, see the following topics:

-   [Upload callback]()
-   [OSS 403]()
-   [PostObject]()
-   [OSS permission]()
-   [OSS CORS]()
-   [Referer]()
-   [STS common errors and troubleshooting]()

For more information about the common errors that you may encounter when you use OSS SDKs and tools, and the troubleshooting methods of these errors, see the following topics:

-   SDK for Java: [FAQ](/intl.en-US/SDK Reference/Java/FAQ.md)
-   SDK for Node.js: [FAQ](/intl.en-US/SDK Reference/Node. js/FAQ.md)
-   [ossutil](https://help.aliyun.com/document_detail/101135.html)
-   [ossfs](/intl.en-US/Tools/ossfs/FAQ.md)
-   [ossftp](/intl.en-US/Tools/ossftp/FAQ.md)

## Appendix: Unsupported parameters in supported operations

If parameters not supported by OSS are added to the request for the object supported by OSS \(for example, the If-Modified-Since parameter is added to the PUT operation request\), OSS returns 400 Bad Request.

Sample error request

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2016 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2016 01:43:51 GMT
Content-Length: 363
```

Sample response

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2016 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955187E
x-oss-server-time: 0
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented. </Message>
  <RequestId>57ABD896CCB80C366955****</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

