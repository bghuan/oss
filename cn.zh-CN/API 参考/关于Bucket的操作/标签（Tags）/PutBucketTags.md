# PutBucketTags

PutBucketTags接口用来给某个存储空间（Bucket）添加或修改标签。

## 注意事项

使用PutBucketTags接口时，有如下注意事项：

-   只有Bucket的拥有者及授权RAM账户才能为Bucket设置用户标签，否则返回403 Forbidden错误，错误码为AccessDenied。
-   最多可设置20对Bucket用户标签（Key-Value对）。
-   PutBucketTags是覆盖语义，即新添加的标签会完全覆盖已有的标签。

## 请求语法

```
PUT /?tagging HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Authorization: SignatureValue
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<Tagging>
  <TagSet>
    <Tag>
      <Key>key1</Key>
      <Value>value1</Value>
    </Tag>
    <Tag>
      <Key>key2</Key>
      <Value>value2</Value>
    </Tag>
  </TagSet>
</Tagging>
```

## 请求头

此接口仅涉及公共请求头，详情请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必需|描述|
|--|--|----|--|
|Tagging|容器|是|设置Bucket TagSet的容器。子元素：TagSet

父元素：无 |
|TagSet|容器|是|包含一系列Bucket Tag的容器。子元素：Tag

父元素：Tagging |
|Tag|容器|是|设置Bucket Tag的容器。子元素：Key、Value

父元素：TagSet |
|Key|字符串|是|指定Bucket Tag的Key。 -   最大长度为64字节。
-   不能以`http ://`、`https://`、`Aliyun`为前缀。
-   必须为UTF-8编码；
-   不能为空。

子元素：无

父元素：Tag |
|Value|字符串|否|指定Bucket Tag的Value。 -   最大长度为128字节。
-   必须为UTF-8编码。
-   可以为空。

子元素：无

父元素：Tag |

## 响应头

此接口仅涉及公共响应头，详情请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
    PUT /?tagging
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 20 Dec 2018 11:49:13 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    <Tagging>
      <TagSet>
        <Tag>
          <Key>testa</Key>
          <Value>testv1</Value>
        </Tag>
        <Tag>
          <Key>testb</Key>
          <Value>testv2</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

-   返回示例

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5C1B138A109F4E405B2D****
    date: Thu, 20 Dec 2018 11:59:06 GMT
    x-oss-server-time: 148
    connection: keep-alive
    ```


