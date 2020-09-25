# DeleteBucketCors

DeleteBucketCors用于关闭指定存储空间（Bucket）对应的跨域资源共享CORS（Cross-Origin Resource Sharing）功能并清空所有规则。

## 请求语法

```
DELETE /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

此接口仅使用公共请求头部，详情请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅返回公共响应头部，详情请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

```
DELETE /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOME****
```

返回示例

```
HTTP/1.1 204 No Content 
x-oss-request-id: 5051845BC4689A033D00****
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/跨域资源共享.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/跨域资源共享.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/跨域资源共享.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/跨域资源共享.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/存储空间/跨域资源共享.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/跨域资源共享.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/跨域资源共享.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/跨域资源共享.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/跨域资源共享.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有删除权限。只有Bucket的拥有者才有权限删除Bucket对应的CORS规则。|

