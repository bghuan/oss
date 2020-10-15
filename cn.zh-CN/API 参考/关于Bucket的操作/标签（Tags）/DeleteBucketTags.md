# DeleteBucketTags

DeleteBucketTags接口用于删除存储空间（Bucket）标签。

**说明：** 如果目标Bucket没有任何标签或指定标签的Key不存在，则返回HTTP状态码204。

## 请求语法

```
DELETE /?tagging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

此接口仅涉及公共请求头，详情请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及公共响应头，详情请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   示例1：删除所有标签

    ```
    DELETE /?tagging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    返回示例

    ```
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A****
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```

-   示例2：删除Key为k1和k2的标签

    ```
    DELETE /?tagging=k1,k2 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    返回示例

    ```
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```


## SDK

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/存储空间标签.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/存储空间标签.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/存储空间标签.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/存储空间/存储空间标签.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/存储空间标签.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/存储空间标签.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/存储空间标签.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|NoSuchBucket|404 No Content|目标Bucket不存在。|
|AccessDenied|403 Forbidden|无权限删除该Bucket的标签信息。只有Bucket的拥有者才能删除Bucket的标签信息。|

