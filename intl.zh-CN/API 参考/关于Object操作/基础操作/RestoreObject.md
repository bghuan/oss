# RestoreObject

RestoreObject接口用于解冻归档类型（Archive）或冷归档（Cold Archive）的文件（Object）。

## 版本控制

Object的各个版本可以对应不同的存储类型。RestoreObject接口默认解冻Object的当前版本，允许通过指定versionId的方式来解冻指定版本的Object。

**说明：**

-   RestoreObject接口只针对归档或冷归档类型的Object，不适用于标准类型和低频访问类型的Object。
-   如果针对该Object第一次调用RestoreObject接口，返回202。
-   如果已经成功调用过RestoreObject接口，且Object已完成解冻，再次调用时返回200 OK。

## 解冻过程说明

-   解冻归档类型Object

    归档类型的Object在执行解冻前后的状态变换过程如下：

    1.  归档类型的Object初始时处于冷冻状态。
    2.  提交一次解冻请求后，Object处于解冻中的状态，完成解冻任务通常需要1分钟。
    3.  服务端完成解冻任务后，Object进入解冻状态，此时您可以读取Object。解冻状态默认持续24小时，24小时内再次调用RestoreObject接口则解冻状态会自动延长24小时。对于同份归档文件，一次解冻流程内可有效调用7次RestoreObject接口达到最长7天的解冻持续时间。
    4.  解冻状态结束后，Object再次返回到冷冻状态。
-   解冻冷归档类型Object

    冷归档类型的Object在执行解冻前后的状态变换过程如下：

    1.  冷归档类型的Object初始时处于冷冻状态。
    2.  提交一次解冻请求后，Object处于解冻中的状态。不同优先级的首字节取回时间如下：
        -   高优先级（Expedited）：表示1小时内完成解冻。
        -   标准（Standard）：表示2~5小时内完成解冻。如果不传入JobParameters节点，则默认为Standard。
        -   批量（Bulk）：表示5~12小时内完成解冻。
    3.  服务端完成解冻任务后，Object进入解冻状态，此时您可以读取Object。您可以指定解冻的天数，最短是1天，最长是7天。
    4.  解冻状态结束后，Object再次返回到冷冻状态。

## 计费说明

状态变换过程中产生的相关费用如下：

-   对处于冷冻状态的Object执行解冻操作，会产生数据取回费用。
-   解冻状态最多延长7天。在此期间不再重复收取数据取回费用。
-   解冻状态结束后，Object又回到冷冻状态，再次执行解冻操作会收取数据取回费用。

## 请求语法

```
POST /ObjectName?restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例

-   首次对处于冷冻状态的归档类型Object提交解冻请求

    请求示例

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Authorization: OSS e1Unnbm1rg****:y4eyu+4yje5ioRCr****
    ```

    返回示例

    ```
    HTTP/1.1 202 Accepted
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```

-   对处于解冻中状态的归档类型Object提交解冻请求

    请求示例

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rg****:21qtGJ+ykDVmdy4eyu+N****
    ```

    返回示例

    ```
    HTTP/1.1 409 Conflict
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Content-Length: 556
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>RestoreAlreadyInProgress</Code>
      <Message>The restore operation is in progress.</Message>
      <RequestId>58EAF141461FB42C2B000008</RequestId>
      <HostId>10.101.200.***</HostId>
    </Error>
    ```

-   对处于解冻状态的归档类型Object提交解冻请求

    请求示例

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rg****:u6O6FMJnn+WuBwbByZxm1+y4eyu+N****
    ```

    返回示例

    ```
    HTTP/1.1 200 Ok
    Date: Sat, 15 Apr 2017 07:45:30 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```

-   对处于解冻状态的冷归档类型Object提交解冻请求

    请求示例

    ```
    POST /coldarchiveobject?restore HTTP/1.1
    Host: cold-archive-bucket.oss-cn-hangzhou.aliyuncs.com
    User-Agent: aliyun-sdk-go/v2.1.0 (Darwin/17.5.0/x86_64;go1.11.8)/ossutil-v1.6.12
    Content-Length: 99
    Authorization: OSS LTAI4FjmjjhjiYK6kMaV****:Gi1x7YHqTw+NQCJo0fKBHcYQ****
    Content-Type: text/plain; charset=utf-8
    Date: Tue, 21 Apr 2020 11:09:19 GMT
    Accept-Encoding: gzip
    <RestoreRequest>
      <Days>2</Days>
      <JobParameters>
        <Tier>Standard</Tier>
      </JobParameters>
    </RestoreRequest>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 21 Apr 2020 11:09:19 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5E9ED45F093E2F3930318EA0
    x-oss-object-restore-priority: Standard
    x-oss-server-time: 10
    ```

-   指定versionId来解冻指定版本的Object

    请求示例

    ```
    POST /oss.jpg?restore&versionId=CAEQNRiBgMClj7qD0BYiIDQ5Y2QyMjc3NGZkODRlMTU5M2VkY2U3MWRiNGRh**** HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:50:48 GMT
    Authorization: OSS o3shiyktjw1****:2JND5qqlAlaA1/kLO4kBbGTw****
    ```

    返回示例

    ```
    HTTP/1.1 202 Accepted
    Date: Tue, 09 Apr 2019 06:50:48 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-version-id: CAEQNRiBgMClj7qD0BYiIDQ5Y2QyMjc3NGZkODRlMTU5M2VkY2U3MWRiNGRh****
    x-oss-request-id: 5CAC40C8B7AEADE017000653
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/管理文件/解冻文件.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/管理文件/解冻文件.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/管理文件/解冻归档文件.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/管理文件/解冻文件.md)
-   [C](/intl.zh-CN/SDK 示例/C/管理文件/解冻归档文件.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/管理文件/解冻文件.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchKey|404|目标Object不存在。|
|OperationNotSupported|400|目标Object不支持解冻，该Object不是归档或冷归档类型。|
|RestoreAlreadyInProgress|409|您已经成功调用过RestoreObject接口，且服务端正在执行解冻操作。请不要重复提交RestoreObject。|

