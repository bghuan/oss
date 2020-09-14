# PutBucketLogging

PutBucketLogging接口用于为存储空间（Bucket）开启访问日志记录功能。访问日志记录功能开启后，OSS将自动记录访问Bucket请求的详细信息，并以小时为单位将访问日志作为一个文件（Object）写入指定的Bucket。

**说明：**

-   源Bucket被删除时，对应的日志规则也将被删除。
-   OSS以小时为单位生成Bucket访问的LOG文件，但并不表示这个小时的所有请求都记录在这个小时的LOG文件内，也有可能出现在上一个或者下一个LOG文件中。访问日志记录功能不保证记录每一条访问信息。
-   OSS生成一个Bucket访问的LOG文件，算作一次PUT操作，并记录其占用的空间，但不会记录产生的流量。LOG生成后，您可以按照普通的Object来操作这些LOG文件。
-   OSS会忽略掉所有以 “x-” 开头的query-string参数，但这个query-string会被记录在访问LOG中。如果你想从海量的访问日志中，标示一个特殊的请求，可以在URL中添加一个 “x-” 开头的query-string参数。例如：

    `http://oss-example.regionid.example.com/aliyun-logo.png`

    `http://oss-example.regionid.example.com/aliyun-logo.png?x-user=admin`

    OSS处理上面两个请求，结果是一样的。但是在访问LOG时，你可以通过搜索“x-user=admin”定位出该请求。


## 请求语法

```
PUT /?logging HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<BucketLoggingStatus>
    <LoggingEnabled>
        <TargetBucket>TargetBucket</TargetBucket>
        <TargetPrefix>TargetPrefix</TargetPrefix>
    </LoggingEnabled>
</BucketLoggingStatus>
```

## 请求元素

**说明：** 访问日志记录功能不支持匿名访问，必须带签名。

|名称|类型|是否必需|描述|
|--|--|----|--|
|BucketLoggingStatus|容器|是|访问日志状态信息的容器。

 子元素：LoggingEnabled

 父元素：无 |
|LoggingEnabled|容器|否|访问日志信息的容器。此元素在开启时需要，关闭时不需要。

 子元素：TargetBucket, TargetPrefix

 父元素：BucketLoggingStatus |
|TargetBucket|字符串|在开启访问日志的时候必选|指定存储访问日志的Bucket。

 **说明：**

-   目标Bucket和源Bucket必须属于同一地域。
-   源Bucket和目标Bucket可以是同一个Bucket，也可以是不同的Bucket；用户也可以将多个源Bucket的日志都保存在同一个目标Bucket内（建议指定不同的 TargetPrefix）。

 子元素：无

 父元素：BucketLoggingStatus.LoggingEnabled |
|TargetPrefix|字符串|否|指定最终被保存的访问日志文件前缀。可以为空。

 子元素：无

 父元素：BucketLoggingStatus.LoggingEnabled |

## 存储访问日志记录的Object命名规则

命名格式：

```
<TargetPrefix><SourceBucket>-YYYY-mm-DD-HH-MM-SS-UniqueString
```

命名参数说明：

|参数|描述|
|:-|:-|
|TargetPrefix|指定Object的前缀。|
|YYYY-mm-DD-HH-MM-SS|该Object被创建的时间。分别填入年、月、日、小时、分钟和秒。例如`2012-09-10-04-00-00`。|
|UniqueString|OSS为访问日志生成的UUID，用于唯一标识该文件。|

Object名称示例：

```
MyLog-oss-example-2012-09-10-04-00-00-0000
```

上述示例中，MyLog-表示用户指定的Object前缀；oss-example表示源Bucket的名称；2012-09-10-04-00-00表示该Object被创建时的北京时间；0000表示OSS系统生成的字符串。

## LOG文件格式

**说明：**

-   OSS的LOG中的任何一个字段，都可能出现 “-”，用于表示未知数据或对于当前请求该字段无效。
-   根据需求，OSS的LOG格式将来会在尾部添加一些字段，请在开发LOG处理工具时考虑兼容性问题。

|名称|例子|含义|
|:-|:-|:-|
|Remote IP|119.xxx.xx.11|请求发起的 IP 地址（Proxy 代理或用户防火墙可能会屏蔽该字段）|
|Reserved|-|保留字段|
|Reserved|-|保留字段|
|Time|\[02/May/2012:00:00:04 +0800\]|OSS 收到请求的时间|
|Request-URI|“GET /aliyun-logo.png HTTP/1.1”|用户请求的 URI（包括 query-string）|
|HTTP Status|200|OSS 返回的 HTTP 状态码|
|SentBytes|5576|用户从 OSS 下载的流量|
|RequestTime \(ms\)|71|完成本次请求的时间（毫秒）|
|Referer|`http://www.aliyun.com/product/oss`|请求的 HTTP Referer|
|User-Agent|curl/7.15.5|HTTP 的 User-Agent 头|
|HostName|oss-example.oss-cn-hangzhou.aliyuncs.com|请求访问域名|
|Request ID|505B016950xxxxxx032593A4|用于唯一标识该请求的 UUID|
|LoggingFlag|true|是否开启了访问日志功能|
|Requester Aliyun ID|16571xxxxxx83691|RAM User ID；匿名访问时为“-”|
|Operation|GetObject|请求类型，例如针对 Bucket 或 Object 的操作请求|
|Bucket|oss-example|请求访问的 Bucket 名字|
|Key|/aliyun-logo.png|用户请求的 Key|
|ObjectSize|5576|Object 大小|
|Server Cost Time \(ms\)|17|OSS 服务器处理本次请求所花的时间（毫秒）|
|Error Code|NoSuchBucket|OSS 返回的错误码|
|Request Length|302|用户请求的长度（Byte）|
|UserID|16571xxxxxx83691|Bucket 拥有者 ID|
|Delta DataSize|280|Bucket 大小的变化量；若没有变化为`-`|
|Sync Request|-|是否是 CDN 回源请求；若不是则为`-`|
|StorageClass|Standard|显示当前 Object 的存储类型，当前取值为 `Standard`、`IA`、`Archive`、`-`，`-`表示操作的是Bucket等无法获取相应存储类型的情况|
|TargetStorageClass|Standard|显示通过 Lifecycle 或 CopyObject 转换存储类型后的目标 Object 存储类型，当前取值为`Standard`、`IA`、`Archive`、`-`，`-`表示该操作无目标 Object 存储类型信息（未通过Lifecycle或者CopyObject转换存储类型）|
|oss\_acc\_src\_oms\_region|us-east-1|显示传输加速的接入地域，例如请求是从美东地区接入，这里的值为 `us-east-1`。若传输加速未开通，或接入地域与 Bucket 所在地域相同，此项为`-`。|

## 示例

-   开启Bucket访问日志的请求示例

    ```
    PUT /?logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 186
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketLoggingStatus>
    <LoggingEnabled>
    <TargetBucket>doc-log</TargetBucket>
    <TargetPrefix>MyLog-</TargetPrefix>
    </LoggingEnabled>
    </BucketLoggingStatus>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88***8906008B
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   关闭Bucket访问日志的请求示例

    **说明：** 当关闭一个Bucket的访问日志记录功能时，只要发送一个空的BucketLoggingStatus即可，具体方法可以参考以下示例。

    ```
    PUT /?logging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Type: application/xml
    Content-Length: 86
    Date: Fri, 04 May 2012 04:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:KU5h8YMUC78M30dXqf3JxrTZHiA=
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketLoggingStatus>
    </BucketLoggingStatus>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674***A4D8906008B
    Date: Fri, 04 May 2012 04:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/访问日志.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/访问日志.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/访问日志.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/访问日志.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/访问日志.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/访问日志.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/访问日志.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/访问日志.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|源Bucket不存在。源Bucket和目标Bucket必须属于同一用户。|
|InvalidTargetBucketForLogging|400|源Bucket和目标Bucket不属于同一个数据中心。|
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致会返回此错误码。|
|MalformedXML|400|请求中的XML不合法。|
|InvalidTargetBucketForLogging|403|请求发起者不是目标 Bucket（TargetBucket）的拥有者。|
|AccessDenied|403|请求发起者不是源 Bucket（BucketName）的拥有者。|

