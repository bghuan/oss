# InitiateMultipartUpload

使用Multipart Upload模式传输数据前，必须先调用该接口来通知OSS初始化一个Multipart Upload事件。

**说明：**

-   该接口会返回一个OSS服务器创建的全局唯一的Upload ID，用于标识本次Multipart Upload事件。您可以根据这个ID来发起相关的操作，如中止Multipart Upload、查询Multipart Upload等。
-   初始化Multipart Upload请求，并不影响已存在的同名Object。
-   该操作计算认证签名时，需要加“?uploads”到CanonicalizedResource中。

## 请求语法

```
POST /ObjectName?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT date
Authorization: SignatureValue
```

## 请求参数

Initiate Multipart Upload时，可以通过encoding-type对返回结果中的Key进行编码。

|名称|类型|描述|
|:-|:-|:-|
|encoding-type|字符串|指定对返回的Key进行编码，目前支持url编码。Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，例如ascii值从0到10的字符。对于Key中包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Key进行编码。 默认值：无

可选值：url |

## 请求头

**说明：** 初始化Multipart Upload请求，支持如下标准的HTTP请求头：Cache-Control、Content-Disposition、Content-Encoding、Content-Type、Expires，以及以`x-oss-meta-`开头的用户自定义Headers。具体含义请参见[PutObject](/cn.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)。

|名称|类型|描述|
|:-|:-|:-|
|Cache-Control|字符串|指定该Object被下载时的网页的缓存行为；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Disposition|字符串|指定该Object被下载时的名称；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Content-Encoding|字符串|指定该Object被下载时的内容编码格式；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|Expires|整数|过期时间（milliseconds）；更详细描述请参照[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 默认值：无 |
|x-oss-forbid-overwrite|字符串|指定InitiateMultipartUpload操作时是否覆盖同名Object。 -   不指定x-oss-forbid-overwrite时，默认覆盖同名Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object；指定x-oss-forbid-overwrite为false时，表示允许覆盖同名Object。

**说明：**

-   当目标Bucket的版本控制状态为“开启”或“暂停”时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。
-   设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求Header（QPS \> 1000），请工单联系我们进行确认，避免影响您的业务。 |
|x-oss-server-side-encryption|字符串|指定上传该Object每个part时使用的服务器端加密编码算法，OSS会对上传的每个part采用服务器端加密编码进行存储。 合法值：AES256或KMS

**说明：** 您需要在控制台开通密钥管理服务KMS后，才可使用KMS加密算法。 |
|x-oss-server-side-encryption-key-id|字符串|表示KMS托管的用户主密钥。 该参数在x-oss-server-side-encryption为KMS时有效。 |
|x-oss-storage-class|字符串|指定Object的存储类型。 取值： Standard、IA、Archive或ColdArchive

支持的接口：PutObject、InitMultipartUpload、AppendObject、 PutObjectSymlink、CopyObject。

**说明：**

-   如果StorageClass的值不合法，返回400 错误。错误码：InvalidArgumet。
-   对于任意存储类型Bucket，若上传Object时指定该值，则此次上传的Object将存储为指定的类型。例如，在IA类型的Bucket中上传Object时，若指定x-oss-storage-class为Standard，则该Object直接存储为Standard类型。 |
|x-oss-tagging|字符串|指定Object的标签，可同时设置多个标签，例如： TagA=A&TagB=B **说明：** Key和Value需要先进行URL编码，如果某项没有`=`，则看作Value为空字符串。 |

## 响应元素

**说明：** 服务器收到初始化Multipart Upload请求后，会返回一个XML格式的请求体。该请求体内包含Bucket、Key和UploadID三个元素。

|名称|类型|描述|
|:-|:-|:-|
|Bucket|字符串|初始化一个Multipart Upload事件的Bucket名称。 父节点：InitiateMultipartUploadResult |
|InitiateMultipartUploadResult|容器|保存Initiate Multipart Upload请求结果的容器。 子节点：Bucket, Key, UploadId

父节点：None |
|Key|字符串|初始化一个Multipart Upload事件的Object名称。 父节点：InitiateMultipartUploadResult |
|UploadId|字符串|唯一标识此次Multipart Upload事件的ID。 父节点：InitiateMultipartUploadResult

**说明：** 请记录下其中的UploadID，以用于后续的Multipart相关操作。 |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，那返回的结果会对Key进行编码。 父节点：容器 |

## 示例

-   请求示例

    ```
    POST /multipart.data?uploads HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Wed, 22 Feb 2012 08:32:21 GMT 
    x-oss-storage-class: Archive
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:/cluRFtRwMTZpC2hTj4F67AG****
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    Content-Length: 230
    Server: AliyunOSS
    Connection: keep-alive
    x-oss-request-id: 42c25703-7503-fbd8-670a-bda01eae****
    Date: Wed, 22 Feb 2012 08:32:21 GMT
    Content-Type: application/xml
    <?xml version="1.0" encoding="UTF-8"?>
    <InitiateMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Bucket> multipart_upload</Bucket>
        <Key>multipart.data</Key>
        <UploadId>0004B9894A22E5B1888A1E29F823****</UploadId>
    </InitiateMultipartUploadResult>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/上传文件/分片上传.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/上传文件/分片上传.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/上传文件/分片上传.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/上传文件/分片上传.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/上传文件/分片上传.md)
-   [C](/cn.zh-CN/SDK 示例/C/上传文件/分片上传.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/上传文件/分片上传.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/上传文件/分片上传.md)
-   [Browser.js](/cn.zh-CN/SDK 示例/Browser.js/上传文件.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidEncryptionAlgorithmError|400|指定OSS服务器端除AES256和KMS以外的加密方式。|
|InvalidArgument|400|在上传每个part时继续添加x-oss-server-side-encryption请求头。|
|KmsServiceNotEnabled|403|使用KMS加密算法时，没有在控制台开通密钥管理服务KMS。|
|FileAlreadyExists|409|当请求的header中携带x-oss-forbid-overwrite=true时，表示禁止覆盖同名文件。如果文件已存在，则返回此错误。|

