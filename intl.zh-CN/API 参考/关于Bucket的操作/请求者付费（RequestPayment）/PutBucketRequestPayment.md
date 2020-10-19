# PutBucketRequestPayment

PutBucketRequestPayment接口用于设置请求者付费模式。

## 注意事项

使用请求者付费模式时，有如下注意事项：

-   不允许匿名访问

    如果您在Bucket上启用了请求者付费模式，则不允许匿名访问该Bucket。请求方必须提供身份验证信息，以便OSS能够识别请求方，从而对请求方而非Bucket拥有者收取请求所产生的费用。

    当请求者是通过扮演阿里云RAM角色来请求数据时，该角色所属的账户将为此请求付费。

-   申请方需携带x-oss-request-payer信息

    如果您在Bucket上启用了请求者付费模式，请求方必须在其请求中包含x-oss-request-payer:requester（在POST、GET和HEAD请求的Header信息中），以表明请求方已知悉请求和数据下载将产生费用。否则，请求方无法通过验证。

    数据拥有者访问该Bucket时，可以不携带x-oss-request-payer请求头。数据拥有者作为请求者访问该Bucket时，请求产生的费用由数据拥有者（也是请求者）来支付。


## 请求语法

```
PUT /?requestPayment HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue 
<?xml version="1.0" encoding="UTF-8"?>
<RequestPaymentConfiguration>
  <Payer>Requester</Payer>
</RequestPaymentConfiguration>
```

## 请求头

此接口仅使用公共请求头部，详情请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必选|描述|
|--|--|----|--|
|RequestPaymentConfiguration|容器|是|请求付费配置的容器。 子节点：Payer |
|Payer|字符串|是|指定Bucket付费类型。 取值：

-   BucketOwner：由Bucket拥有者付费。
-   Requester：由请求者付费。

、

父节点：RequestPaymentConfiguration |

## 响应头

此接口仅返回公共响应头部，详情请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

```
PUT /?requestPayment
Content-Length: 83
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Tue, 23 Jul 2019 01:33:47 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:77Dvh5wQgIjWjwO/KyRt8dOP****
<RequestPaymentConfiguration>
  <Payer>Requester</Payer>
</RequestPaymentConfiguration>
```

返回示例

```
200 (OK)
content-length: 0
x-oss-request-id: 5D3663FBB007B79097FC****
date: Tue, 23 Jul 2019 01:33:47 GMT
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/存储空间/请求者付费模式.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/存储空间/请求者付费模式.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/存储空间/请求者付费模式.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/存储空间/请求者付费模式.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/存储空间/请求者付费模式.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/存储空间/请求者付费模式.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|访问的Bucket不存在。|

