# GetBucketV2 \(ListObjectsV2\)

GetBucketV2 \(ListObjectsV2\)接口用于列举存储空间（Bucket）中所有文件（Object）的信息。

**说明：** 执行GetBucketV2 \(ListObjectsV2\)请求时不会返回Object中自定义的元信息。

## 请求语法

```
GET /?list-type=2 HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求参数

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|List-type|数字|是|取值只能为2。|
|Delimiter|字符串|否|对Object名字进行分组的字符。所有Object名字包含指定的前缀，第一次出现Delimiter字符之间的Object作为一组元素（即CommonPrefixes）。 默认值：无 |
|Start-after|字符串|否|设定从Start-after之后按字母排序开始返回Object。 Start-after用来实现分页显示效果，参数的长度必须小于1024字节。

做条件查询时，即使Start-after在列表中不存在，也会从符合Start-after字母排序的下一个开始打印。

默认值：无 |
|Continuation-token|字符串|否|指定list操作需要从此token开始。您可从ListObjectsV2结果中的NextContinuationToken获取此token。|
|Max-keys|字符串|否|指定返回Object的最大数。 取值：大于0小于1000

默认值：100

**说明：** 如果因为Max-keys的设定无法一次完成列举，返回结果会附加一个`<NextContinuationToken>`作为下一次列举的Continuation-token。 |
|Prefix|字符串|否|限定返回文件的Key必须以Prefix作为前缀。 如果把Prefix设为某个文件夹名，则列举以此Prefix开头的文件，即该文件夹下递归的所有文件和子文件夹。

在设置Prefix的基础上，将Delimiter设置为正斜线（/）时，返回值就只列举该文件夹下的文件，文件夹下的子文件夹名返回在CommonPrefixes中，子文件夹下递归的所有文件和文件夹不显示。

例如，一个Bucket中有三个Object ，分别为fun/test.jpg、 fun/movie/001.avi和fun/movie/007.avi。若设定Prefix为fun/，则返回三个Object；如果在Prefix设置为fun/的基础上，将Delimiter设置为正斜线（/），则返回fun/test.jpg和fun/movie/。

**说明：**

-   参数的长度必须小于1024字节。
-   使用Prefix查询时，返回的Key中仍会包含Prefix。

默认值：无 |
|Encoding-type|字符串|否|对返回的内容进行编码并指定编码的类型。 默认值：无

可选值：url

**说明：** Delimiter、Start-after、Prefix、NextContinuationToken以及Key使用UTF-8字符。如果Delimiter、Start-after、Prefix、NextContinuationToken以及Key中包含XML 1.0标准不支持的控制字符，您可以通过指定Encoding-type对返回结果中的Delimiter、Start-after、Prefix、NextContinuationToken以及Key进行编码。 |
|Fetch-owner|布尔值|否|指定是否在返回结果中包含owner信息。 合法值：true、false

-   true：表示返回结果中包含owner信息。
-   false：表示返回结果中不包含owner信息。

默认值：false |

## 响应元素

|名称|类型|描述|
|--|--|--|
|Contents|容器|保存每个返回Object元信息的容器。 父节点：ListBucketResult |
|CommonPrefixes|字符串|如果请求中指定了Delimiter参数，则会在返回的响应中包含CommonPrefixes元素。该元素表明以Delimiter结尾，并有共同前缀的Object名称的集合。 父节点：ListBucketResult |
|Delimiter|字符串|对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现Delimiter字符之间的Object作为一组元素CommonPrefixes。 父节点：ListBucketResult |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了Encoding-type，则会对返回结果中的Delimiter、StartAfter、Prefix、NextContinuationToken和Key这些元素进行编码。 父节点：ListBucketResult |
|DisplayName|字符串|Object拥有者名称。 父节点：ListBucketResult.Contents.Owner |
|ETag|字符串|ETag在每个Object生成时创建，用于标识一个Object的内容。 父节点：ListBucketResult.Contents

-   对于PutObject请求创建的Object，ETag值是其内容的MD5值。
-   对于其他方式创建的Object，ETag值是其内容的UUID。
-   ETag值可以用于检查Object内容是否发生变化。不建议使用ETag值作为Object内容的MD5校验数据完整性的依据。 |
|ID|字符串|Bucket拥有者的用户ID。 父节点：ListBucketResult.Contents.Owner |
|IsTruncated|枚举字符串|请求中返回的结果是否被截断。 返回值：true、false

-   true表示本次没有返回全部结果。
-   false表示本次已经返回了全部结果。

父节点：ListBucketResult |
|Key|字符串|Object的Key。 父节点：ListBucketResult.Contents |
|LastModified|时间|Object最后被修改的时间。 父节点：ListBucketResult.Contents |
|ListBucketResult|容器|保存GetBucket请求结果的容器。 子节点：Name、Prefix、StartAfter、MaxKeys、 Delimiter、IsTruncated、NextContinuationToken、Contents

父节点：None |
|StartAfter|字符串|如果请求中指定了StartAfter参数，则会在返回的响应中包含StartAfter元素。|
|MaxKeys|字符串|响应请求内返回结果的最大数目。 父节点：ListBucketResult |
|Name|字符串|Bucket名称。 父节点：ListBucketResult |
|Owner|容器|保存Bucket拥有者信息的容器。 子节点：DisplayName、ID

父节点：ListBucketResult |
|Prefix|字符串|本次查询结果的前缀。 父节点：ListBucketResult |
|Size|字符串|Object的字节数。 父节点：ListBucketResult.Contents |
|StorageClass|字符串|Object的存储类型。 父节点：ListBucketResult.Contents |
|ContinuationToken|字符串|如果请求中指定了ContinuationToken参数，则会在返回的响应中包含ContinuationToken元素。|
|KeyCount|数字|此次请求返回的Key的个数。如果指定了Delimiter，则KeyCount为Key和CommonPrefixes的元素之和。|
|NextContinuationToken|字符串|表明此次GetBucketV2 \(ListObjectsV2\)请求包含后续结果，需要将NextContinuationToken指定为ContinuationToken继续获取结果。|

## 示例

-   简单请求示例

    ```
    GET /?list-type=2 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <MaxKeys>100</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a</Key>
            <LastModified>2020-05-18T05:45:43.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>b</Key>
            <LastModified>2020-05-18T05:45:50.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>b/c</Key>
            <LastModified>2020-05-18T05:45:54.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>bc</Key>
            <LastModified>2020-05-18T05:45:59.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>c</Key>
            <LastModified>2020-05-18T05:45:57.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <KeyCount>6</KeyCount>
    </ListBucketResult>
    ```

-   带Prefix参数的请求示例

    ```
    GET /?list-type=2&prefix=a HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix>a</Prefix>
        <MaxKeys>100</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a</Key>
            <LastModified>2020-05-18T05:45:43.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <KeyCount>2</KeyCount>
    </ListBucketResult>
    ```

-   带Prefix和Delimiter参数的请求示例

    ```
    GET /?list-type=2&prefix=a/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix>a/</Prefix>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <CommonPrefixes>
            <Prefix>a/b/</Prefix>
        </CommonPrefixes>
        <KeyCount>2</KeyCount>
    </ListBucketResult>
    ```

-   带StartAfter、MaxKeys和FetchOwner参数的请求示例

    ```
    GET /?list-type=2&start-after=b&max=keys=3&fetch-owner=true HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <StartAfter>b</StartAfter>
        <MaxKeys>3</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>true</IsTruncated>
        <NextContinuationToken>CgJiYw--</NextContinuationToken>
        <Contents>
            <Key>b/c</Key>
            <LastModified>2020-05-18T05:45:54.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>ba</Key>
            <LastModified>2020-05-18T11:17:58.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>bc</Key>
            <LastModified>2020-05-18T05:45:59.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <KeyCount>3</KeyCount>
    </ListBucketResult>
    ```


## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有访问该Bucket的权限。|
|InvalidArgument|400|-   Max-keys小于0或者大于1000。
-   Prefix、Start-after、Delimiter参数的长度不符合要求。 |

