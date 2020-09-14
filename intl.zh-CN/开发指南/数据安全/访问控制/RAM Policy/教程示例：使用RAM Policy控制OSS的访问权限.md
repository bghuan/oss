# 教程示例：使用RAM Policy控制OSS的访问权限

本教程示例详细演示了如何使用RAM Policy控制用户对OSS存储空间（Bucket）、文件夹以及文件夹下文件（Object）的访问。

## 前提条件

使用RAM Policy控制用户对存储空间、文件夹以及文件夹下文件的访问控制前，确保已完成如下事项：

-   已创建存储空间。

    此处以使用阿里云主账号创建一个名为example-company的存储空间为例。有关创建存储空间的详情，请参见[创建存储空间](/intl.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。

-   已上传文件或文件夹。

    在example-company的存储空间中上传一个根目录文件（oss-dg.pdf）以及三个文件夹，分别为Development/、Marketing/和Private/。各文件夹中包含的文件以及各文件在控制台的显示层级如下：

    |文件夹|文件显示层级|
    |---|------|
    |Development/|Development/Alibaba Cloud.pdf、Development/ProjectA.docx、Development/ProjectB.docx|
    |Marketing/|Marketing/data2016.xlsx、Marketing/example2016.txt|
    |Private/|Private/2017/images.zip、Private/2017/promote.pptx|

    有关上传文件的详情请参见[上传文件](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/上传文件.md)。

-   已创建RAM用户

    此处以通过阿里云主账号创建RAM用户Anne和Leo为例。有关创建RAM用户的详情，请参见[创建RAM用户](/intl.zh-CN/用户管理/创建RAM用户.md)。


## 背景信息

RAM Policy是基于用户的授权策略。通过设置RAM Policy，您可以集中管理您的用户（例如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限，例如限制您的用户只拥有对某一个Bucket的读权限。

RAM Policy为JSON格式。各字段定义如下：

-   Statement：授权语句，一个权限策略可以有多条授权语句。
-   Effect：授权效力，包括允许（Allow）和拒绝（Deny）两种。

    **说明：** 当权限策略中既有Allow又有Deny的授权语句时，遵循Deny优先的原则。

-   Action：对具体资源的操作权限。

如果您选择使用RAM Policy，建议您通过官方工具[RAM策略编辑器](/intl.zh-CN/常用工具/RAM策略编辑器.md)快速生成RAM策略。

相比于RAM Policy，Bucket Policy支持在控制台直接进行图形化配置操作，并且Bucket拥有者直接可以进行访问授权。详情请参见[使用Bucket Policy授权其他用户访问OSS资源](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/使用Bucket Policy授权其他用户访问OSS资源.md)。

## 场景与实现

假设您需要为RAM用户Anne和Leo分别授予访问上述不同级别资源的权限：

-   RAM用户Anne仅允许访问Development/文件夹下的所有子文件夹以及文件。详情请参见[步骤2：授予RAM用户Anne特定权限](#section_gqp_p63_kl8)。
-   RAM用户Leo仅允许访问Marketing/文件夹下的所有子文件夹以及文件。详情请参见[步骤3：授予RAM用户Leo特定权限](#section_fvm_dph_z1k)。
-   Private/文件夹访问权限保留为私有，即主账号下的所有RAM用户均不能访问。

结合本示例场景得知RAM用户Anne和Leo需同时具有以下权限：

-   列举主账号下所有存储空间的权限，即oss:ListBuckets的操作权限。
-   列举example-company存储空间中的根目录文件、文件夹和对象的权限，即oss:ListObjects的操作权限。

此时，您可以通过创建用户组对职责相同的RAM用户进行分类并授权，从而更好的管理用户及其权限。有关创建用户组并授予用户组级别权限的详情，请参见[步骤1：创建用户组并授予用户组级别权限](#section_tu8_3br_l9p)。

## 授权前须知

在授予RAM用户相关权限之前，您需要了解单击某个存储空间的名字时控制台向OSS发送请求、OSS返回响应，以及控制台如何解析该响应的逻辑。

-   请求某个存储空间

    单击example-company存储空间时，控制台会将[GetBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)请求发送至OSS。

    -   请求示例

        ```
        GET /?prefix=&delimiter=/ HTTP/1.1
        Host: example-company.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        此请求包括prefix和delimiter参数，其中prefix的值为空字符串，delimiter的值为正斜线（/）。

    -   响应示例

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <?xml version="1.0" encoding="UTF-8"?>
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>example-company</Name>
        <Prefix></Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>oss-dg.pdf</Key>
                ...
            </Contents>
           <CommonPrefixes>
                <Prefix>Development</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Marketing</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Private</Prefix>
           </CommonPrefixes>
        </ListBucketResult>
        ```

    -   控制台解析

        控制台会解析此结果并显示如下的根级别项：

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6647559951/p1189.png)

-   请求存储空间下的某个文件夹

    单击Development/文件夹，控制台会将[GetBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)请求发送至OSS。此请求包括以下参数：

    -   请求示例

        ```
        GET /?prefix=Development/&delimiter=/ HTTP/1.1
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        此请求包括prefix和delimiter参数，其中prefix的值为`Development/`，delimiter的值为正斜线（/）。

    -   响应示例

        作为响应，OSS返回以指定前缀开头的key：

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <?xml version="1.0" encoding="UTF-8"?>
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>example-company</Name>
        <Prefix>Development/</Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>ProjectA.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>ProjectB.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>Alibaba Cloud.pdf</Key>
                ...
            </Contents>
        </ListBucketResult>
        ```

    -   控制台解析

        控制台会解析此结果并显示如下的key：

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6647559951/p1190.png)


## 步骤1：创建用户组并授予用户组级别权限

此处以创建Staff用户组为例，有关创建用户组的详情请参见[创建用户组](/intl.zh-CN/用户组管理/创建用户组.md)。

-   授予RAM用户列举主账号下的所有存储空间的权限
    1.  创建策略。

        本示例中创建名为`AllowGroupToSeeBucketListInConsole`的策略，内容如下：

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                  "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
           "Resource": [
             "acs:oss:*:*:*"
           ]
         }
        ]
        }
        ```

        创建策略步骤请参见[创建自定义策略](/intl.zh-CN/权限策略管理/自定义策略/创建自定义策略.md)。

    2.  将`AllowGroupToSeeBucketListInConsole`策略分配给Staff组。

        详情请参见[为RAM用户授权](/intl.zh-CN/用户管理/为RAM用户授权.md)。

-   授予RAM用户列举存储空间根级别内容的权限

    1.  登录[访问控制RAM管理控制台](https://ram.console.aliyun.com)。
    2.  单击**权限管理** \> **权限策略管理**。
    3.  找到已创建的`AllowGroupToSeeBucketListInConsole`策略，单击策略名称。
    4.  单击**修改策略内容**。

        RAM用户要列举存储空间的内容，需要有`oss:ListObjects`权限。

        ```
        {
           "Version": "1",
           "Statement": [
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                 "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
               "Resource": [
                 "acs:oss:*:*:*"
               ],
               "Condition": {}
             },
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListObjects"
               ],
               "Resource": [
                 "acs:oss:*:*:example-company"
               ],
               "Condition": {
                 "StringLike": {
                   "oss:Prefix": [
                     ""
                   ],
                   "oss:Delimiter": [
                     "/"
                   ]
                 }
               }
             }
           ]
         }
        ```

    **说明：** 您最多可对策略内容修改5次。如果超过了5次，则需要删除该策略并创建一个新的策略，然后再次将新策略分配给Staff用户组。


## 步骤2：授予RAM用户Anne特定权限

以下步骤演示了如何向RAM用户Anne授予访问Development/文件夹下所有文件的权限。

1.  创建一个名为`AllowGroupToSeeBucketListInConsole`的策略，内容如下：

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Development/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",         
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Development/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  将`AllowGroupToSeeBucketListInConsole`策略授予RAM用户Anne。


如果您希望RAM用户Annie不仅可以访问Development/文件夹下的所有文件，还允许向此文件夹中写入文件，则策略内容如下：

```
{
   "Version": "1",
   "Statement": [
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:example-company"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Development/*"
           ]
         }
       }
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:GetObject",
         "oss:PutObject",
         "oss:GetObjectAcl"
       ],
       "Resource": [
         "acs:oss:*:*:example-company/Development/*"
       ],
       "Condition": {}
     }
   ]
 }
```

## 步骤3：授予RAM用户Leo特定权限

以下步骤演示了如何向RAM用户Leo授予访问Marketing/文件夹下所有文件的权限。

1.  创建一个名为`AllowGroupToSeeBucketListInConsole`的策略，内容如下：

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Marketing/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",         
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Marketing/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  将`AllowGroupToSeeBucketListInConsole`策略授予RAM用户Leo。


## 步骤4：拒绝RAM用户访问Private文件夹

在本示例场景中，要求对Private/文件夹访问权限保留为私有，即主账号下的所有RAM用户均不能访问。因此，需要添加一个显式拒绝访问Private/文件夹的策略。显式拒绝策略优先于其他任何权限。具体实现方法如下：

-   添加以下语句，显式拒绝对Private/（example-company/Private/\*）的访问：

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:*"
        ],
        "Resource": [
          "acs:oss:*:*:example-company/Private/*"
        ],
        "Condition": {}
      }
    ```

-   当您请求指定访问Prefix为Private时，拒绝执行`ListObjects`操作的权限。

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:ListObjects"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ],
        "Condition": {
          "StringEquals": {
            "oss:Prefix": [
              "Private/"
            ]
          }
        }
      }
    ```

    指定以上策略后，当RAM用户请求列举Private文件夹下的Private/2017/images.zip、Private/2017/promote.pptx文件时，OSS将返回错误响应。

-   用包含前述拒绝语句的更新策略取代Staff组策略`AllowGroupToSeeBucketListInConsole`。

    在应用更新策略后，组中的任何RAM用户都不能访问您的存储空间中的Private文件夹。

    ```
    {
    "Version": "1",
    "Statement": [
     {
       "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketStat",
             "oss:GetBucketInfo",
             "oss:GetBucketAcl"
           ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:example-company"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
              ""
            ],
           "oss:Delimiter": [
             "/"
           ]
         }
       }
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:*"
       ],
       "Resource": [
         "acs:oss:*:*:example-company/Private/*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Private/"
           ]
         }
       }
     }
    ]
    }
    ```


