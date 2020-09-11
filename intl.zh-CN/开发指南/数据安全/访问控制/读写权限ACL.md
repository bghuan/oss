# 读写权限ACL

OSS为权限控制提供访问控制列表（ACL）。ACL是授予Bucket和Object访问权限的访问策略。 您可以在创建存储空间（Bucket）或上传文件（Object）时配置ACL，也可以在创建Bucket或上传Object后的任意时间内修改ACL。

**说明：** 基于读写权限ACL的OSS API接口详细信息请参考：

-   设置Bucket ACL：[PutBucketACL](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/PutBucketACL.md)
-   获取Bucket ACL：[GetBucketACL](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/GetBucketAcl.md)
-   设置Object ACL：[PutObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/PutObjectACL.md)
-   获取Object ACL：[GetObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/GetObjectACL.md)

## Bucket ACL

-   Bucket ACL介绍

    Bucket ACL是Bucket级别的权限访问控制。目前有三种访问权限：public-read-write、public-read和private，含义如下：

    |权限值|中文名称|权限对访问者的限制|
    |:--|:---|:--------|
    |public-read-write|公共读写|任何人（包括匿名访问）都可以对该Bucket中的Object进行读、写、删除操作；所有这些操作产生的费用由该Bucket的Owner承担，请慎用该权限。|
    |public-read|公共读，私有写|只有该Bucket的Owner或者授权对象可以对存放在其中的Object进行写、删除操作；任何人（包括匿名访问）可以对Object进行读操作。|
    |private|私有读写|只有该Bucket的Owner或者授权对象可以对存放在其中的Object进行读、写、删除操作；其他人在未经授权的情况下无法访问该Bucket内的 Object。|

-   操作方式

    |操作方式|说明|
    |----|--|
    |[控制台](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/修改存储空间读写权限.md)|Web应用程序，直观易用。|
    |[图形化工具ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)|图形化工具，易操作。|
    |[命令行工具ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/set-acl.md)|命令行工具，性能好。|
    |[Java SDK](/intl.zh-CN/SDK 示例/Java/存储空间/管理存储空间访问权限.md)|丰富、完整的各类语言SDK demo。|
    |[Python SDK](/intl.zh-CN/SDK 示例/Python/存储空间/管理存储空间访问权限.md)|
    |[PHP SDK](/intl.zh-CN/SDK 示例/PHP/存储空间/创建存储空间.md)|
    |[Go SDK](/intl.zh-CN/SDK 示例/Go/存储空间/管理存储空间的访问权限.md)|
    |[C SDK](/intl.zh-CN/SDK 示例/C/存储空间/创建存储空间.md)|
    |[.NET SDK](/intl.zh-CN/SDK 示例/.NET/存储空间/创建存储空间.md)|
    |[Node.js SDK](/intl.zh-CN/SDK 示例/Node.js/存储空间/创建存储空间.md)|
    |[Ruby SDK](/intl.zh-CN/SDK 示例/Ruby/存储空间/创建存储空间.md)|


## Object ACL

-   Object ACL介绍

    Object ACL是Object级别的权限访问控制。目前有四种访问权限：private、public-read、public-read-write、default。PutObjectACL操作通过 Put 请求中的`x-oss-object-acl`头来设置，这个操作只有Bucket Owner有权限执行。

    Object ACL的四种访问权限含义如下：

    |权限值|中文名称|权限对访问者的限制|
    |:--|:---|:--------|
    |public-read-write|公共读写|该ACL表明某个Object是公共读写资源，即所有用户拥有对该Object的读写权限。|
    |public-read|公共读，私有写|该ACL表明某个Object是公共读资源，即非Object Owner只有该Object的读权限，而Object Owner拥有该Object的读写权限。|
    |private|私有读写|该ACL表明某个Object是私有资源，即只有该Object的Owner拥有该Object的读写权限，其他的用户没有权限操作该Object。|
    |default|默认权限|该ACL表明某个Object是遵循Bucket读写权限的资源，即Bucket是什么权限，Object就是什么权限。|

    **说明：**

    -   如果没有设置Object的权限，即Object的ACL为default，Object的权限和Bucket权限一致。
    -   如果设置了Object的权限，Object的权限大于Bucket权限。例如，设置了Object的权限是public-read，则无论Bucket是什么权限，该Object都可以被身份验证访问和匿名访问。
-   操作方式

    |操作方式|说明|
    |----|--|
    |[控制台](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/设置文件读写权限ACL.md)|Web应用程序，直观易用。|
    |[图形化工具ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md)|图形化工具，易操作。|
    |[命令行工具ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/set-acl.md)|命令行工具，性能好。|
    |[Java SDK](/intl.zh-CN/SDK 示例/Java/管理文件/管理文件访问权限.md)|丰富、完整的各类语言SDK demo。|
    |[Python SDK](/intl.zh-CN/SDK 示例/Python/管理文件/管理文件访问权限.md)|
    |[PHP SDK](/intl.zh-CN/SDK 示例/PHP/管理文件/管理文件访问权限.md)|
    |[Go SDK](/intl.zh-CN/SDK 示例/Go/管理文件/管理文件访问权限.md)|
    |[C SDK](/intl.zh-CN/SDK 示例/C/管理文件/管理文件访问权限.md)|
    |[.NET SDK](/intl.zh-CN/SDK 示例/.NET/管理文件/管理文件访问权限.md)|


