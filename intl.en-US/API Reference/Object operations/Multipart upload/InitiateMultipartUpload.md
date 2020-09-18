# InitiateMultipartUpload

You must call this operation to require OSS to initiate a multipart upload task before you can transmit data in multipart upload mode.

**Note:**

-   The operation returns a unique upload ID created by the OSS server to identify the multipart upload task. You can initiate operations such as stopping or querying the multipart upload task based on this upload ID.
-   The InitiateMultipartUpload request does not affect existing objects with the same name.
-   When you perform this operation to calculate the signature for authentication, you must add "?uploads" to the CanonicalizedResource header.

## Request syntax

```
POST /ObjectName? uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## Request parameters

The encoding-type parameter can be specified in the InitiateMultipartUpload request. OSS uses the specified encoding type to encode the object name in the response.

|Parameter|Type|Description|
|:--------|:---|:----------|
|encoding-type|String|Specifies the encoding type of the object name in the response. The object name can contain any characters encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse certain control characters, such as characters with an ASCII value from 0 to 10. You can set the encoding-type parameter to encode the object name in the response. Set the value to url. This parameter is empty by default.

 Valid value: url |

## Request headers

**Note:** The InitiateMultipartUpload request supports the standard HTTP request headers Cache-Control, Content-Disposition, Content-Encoding, Content-Type, and Expires, as well as custom headers that start with `x-oss-meta-`. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

|Header|Type|Description|
|:-----|:---|:----------|
|Cache-Control|String|Specifies the web page caching behavior when the object is downloaded. For more information, visit [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). This parameter is empty by default. |
|Content-Disposition|String|Specifies the name of the object when it is downloaded. For more information, visit [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). This parameter is empty by default. |
|Content-Encoding|String|Specifies the content encoding format when the object is downloaded. For more information, visit [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). This parameter is empty by default. |
|Expires|Integer|Specifies the expiration time in milliseconds. For more information, visit [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). This parameter is empty by default. |
|x-oss-forbid-overwrite|String|Specifies whether the InitiateMultipartUpload operation overwrites the object with the same name. -   By default, if x-oss-forbid-overwrite is not specified, the object with the same name is overwritten.
-   If x-oss-forbid-overwrite is set to true, the object with the same name is not overwritten. If x-oss-forbid-overwrite is set to false, the object with the same name is overwritten.

 **Note:**

-   The x-oss-forbid-overwrite request header is invalid when the versioning state of the target bucket is Enabled or Suspended. In this case, the CompleteMultipartUpload operation overwrites the object with the same name.
-   Specifying the x-oss-forbid-overwrite request header may degrade the query per second \(QPS\) performance of OSS. If you want to use the x-oss-forbid-overwrite request header for a large number of operations \(QPS \> 1000\), submit a ticket. |
|x-oss-server-side-encryption|String|Specifies the server-side encryption algorithm used to encrypt each part of the object. Each part is stored in OSS after the server-side encryption. Valid values: AES256 and KMS

 **Note:** You can use Key Management Service \(KMS\) for encryption after you activate KMS in the console. |
|x-oss-server-side-encryption-key-id|String|Specifies the ID of the customer master key \(CMK\) hosted in KMS. This parameter takes effect only when x-oss-server-side-encryption is set to KMS. |
|x-oss-storage-class|String|Specifies the storage class of the object. Valid values: Standard, IA, Archive and ColdArchive

 Supported operations: PutObject, InitiateMultipartUpload, AppendObject, PutObjectSymlink, and CopyObject

 **Note:**

-   If the value of x-oss-storage-class is invalid, 400 is returned with error code InvalidArgumet.
-   If the storage class is specified when you upload the object, the specified storage class applies regardless of the storage class of the bucket that contains the object. If you set x-oss-storage-class to Standard when you upload an object to an IA bucket, the storage class of the object is Standard. |
|x-oss-tagging|String|Specifies the tag for the object. You can set multiple tags for the object. Example: TagA=A&TagB=B. **Note:** The tag key and value must be URL-encoded. If a key-value pair does not contain an equal sign \(=\), the tag value is considered as an empty string. |

## Response elements

**Note:** After receiving the InitiateMultipartUpload request, the server returns a message body that is in XML format. The message body contains the following elements: Bucket, Key, and UploadID.

|Element|Type|Description|
|:------|:---|:----------|
|Bucket|String|The name of the bucket for which the multipart upload task is initiated. Parent node: InitiateMultipartUploadResult |
|InitiateMultipartUploadResult|Container|The container that contains the result of the InitiateMultipartUpload request. Child node: Bucket, Key, and UploadId

 Parent node: none |
|Key|String|The name of the object for which the multipart upload task is initiated. Parent node: InitiateMultipartUploadResult |
|UploadId|String|The unique ID of the multipart upload task. Parent node: InitiateMultipartUploadResult

 **Note:** You need to record the upload ID for subsequent multipart-related operations. |
|EncodingType|String|The encoding type of the object name in the response. If the encoding-type parameter is specified in the request, the object name in the response is encoded. Parent node: Container |

## Examples

-   Sample requests

    ```
    POST /multipart.data? uploads HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Wed, 22 Feb 2012 08:32:21 GMT 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:/cluRFtRwMTZpC2hTj4F67AG****
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    Content-Length: 230
    Server: AliyunOSS
    Connection: keep-alive
    x-oss-request-id: 42c25703-7503-fbd8-670a-bda01eae****
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    Content-Type: application/xml
    <? xml version="1.0" encoding="UTF-8"? >
    <InitiateMultipartUploadResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Bucket> multipart_upload</Bucket>
        <Key>multipart.data</Key>
        <UploadId>0004B9894A22E5B1888A1E29F823****</UploadId>
    </InitiateMultipartUploadResult>
    ```


## SDKs

The SDKs of the InitiateMultipartUpload operation for various programming languages are as follows:

-   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Upload objects/Multipart upload.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [OSS SDK for C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)
-   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)
-   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidEncryptionAlgorithmError|400|The error message returned because the server-side encryption method other than AES 256 or KMS is specified.|
|InvalidArgument|400|The error message returned because the x-oss-server-side-encryption request header is added each time a part is uploaded.|
|KmsServiceNotEnabled|403|The error message returned because KMS is specified as the server-side encryption method but KMS is not activated in the console.|
|FileAlreadyExists|409|The error message returned because an object with the same object name already exists when the request contains an x-oss-forbid-overwrite header and the value of this header is set to true.|

