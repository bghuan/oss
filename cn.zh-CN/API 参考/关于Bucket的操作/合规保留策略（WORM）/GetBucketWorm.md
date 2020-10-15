# GetBucketWorm

GetBucketWorm用于获取指定存储空间（Bucket）的合规保留策略信息。

**说明：** 若指定用来获取Bucket的合规保留策略信息对应的WORM ID不存在，则返回404。

## 注意事项

对象存储OSS支持WORM（Write Once Read Many）特性，允许您以不可删除、不可篡改的方式保存和使用数据。OSS允许针对Bucket设置基于时间的合规保留策略，保护周期为1天到70年。

当合规保留策略锁定后，您可以在Bucket中上传和读取文件（Object），但是在Object的保留时间到期之前，不允许删除Object及合规保留策略。Object的保留时间到期后，才可以删除Object。

## 请求头

此接口仅涉及公共请求头，详情请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅涉及公共响应头，详情请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|描述|
|--|--|--|
|WormConfiguration|容器|根节点。 子节点：WormId、State、RetentionPeriodInDays、CreationDate |
|WormId|字符串|合规保留策略的ID。|
|State|字符串|合规保留策略所处的状态。 可选值：

-   InProgress：合规保留策略创建后，该策略默认处于“InProgress”状态，且该状态的有效期为24小时。
-   Locked：合规保留策略处于锁定状态。 |
|RetentionPeriodInDays|正整数|Object的指定保留天数。|
|CreationDate|字符串|合规保留策略的创建时间。|

## 示例

-   请求示例

    ```
    GET /?worm HTTP/1.1
    Date: Fri, 16 Oct 2020 11:18:32 GMT
    Host: BucketName.oss.aliyuncs.com
    Authorization: SignatureValue
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5374A2880232A65C2300****
    Date: Fri, 16 Oct 2020 11:18:32 GMT
    Content-Type: application/xml
    Content-Length: length
    <WormConfiguration>
      <WormId>1666E2CFB2B3418****</WormId>
      <State>Locked</State>
      <RetentionPeriodInDays>1</RetentionPeriodInDays>
      <CreationDate>2020-10-15T15:50:32</CreationDate>
    </WormConfiguration>
    ```


