# OpenOssService

OpenOssService接口用于开通对象存储OSS服务。开通OSS服务后，才能进行OSS相关操作，例如创建存储空间（Bucket）、上传文件（Object）等。

## 请求参数

|参数|类型|是否必选|描述|
|--|--|----|--|
|Action|字符串|是|操作接口名，系统规定参数。取值：OpenOssService |

## 返回参数

|参数|类型|描述|
|--|--|--|
|RequestId|字符串|请求ID|
|OrderId|字符串|订单号|

## 示例

-   请求示例

    ```
    http(s)://oss-admin.aliyuncs.com/?Action=OpenOssService
    &<公共请求参数>
    ```

-   XML格式返回示例

    ```
    <OpenOssServiceResponse>
          <RequestId>BD35E3F5-C21E-464D-B575-CC4E5FA7****</RequestId>
          <OrderId>20686300099****</OrderId>
    </OpenOssServiceResponse>
    ```

-   JSON格式返回示例

    ```
    {
      "RequestId": "BD35E3F5-C21E-464D-B575-CC4E5FA7****"
    “OrderId”：“20686300099****”
    }
    ```


## 错误码

|HTTP状态码|Code|描述|
|-------|----|--|
|400|SYSTEM.SALE\_VALIDATE\_NO\_SPECIFIC\_CODE\_FAILED|OSS服务已开通，请勿重复开通。|
|400|ORDER.NO\_REAL\_NAME\_AUTHENTICATION|您尚未进行实名认证，不符合购买条件。请登录用户中心进行实名认证。|

