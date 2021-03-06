# OSS文件删除或覆盖后能不能恢复？

OSS后端的冗余机制是针对服务器、硬件等出现故障进行数据恢复。您对OSS的数据手动删除、覆盖或配置自动删除后，阿里云将按照您的指令删除、覆盖数据，并且无法恢复。

## 数据删除、覆盖说明

-   [服务条款](https://help.aliyun.com/document_detail/31821.html)中关于“用户业务数据”的说明内容如下：
    -   您可自行对您的用户业务数据进行删除、更改等操作。如您自行释放服务或删除数据的，阿里云将删除您的数据，按照您的指令不再保留该数据。
    -   用户业务数据一经删除，即不可恢复；您应自行承担数据因此被删除所引发的后果和责任，您理解并同意，阿里云没有继续保留、导出或者返还用户业务数据的义务。
-   [服务等级协议](https://help.aliyun.com/document_detail/60175.html)中关于“数据可销毁性”服务要求如下：

    在用户主动删除数据或用户服务期满后需要销毁数据时，阿里云将自动清除对应物理服务器上磁盘和内存数据，使得数据无法恢复。


## 可能会造成数据删除、覆盖的操作

以下方式可能导致您的数据被删除、覆盖，请慎重操作。

-   通过OSS控制台、命令行工具ossutil、图形化工具ossbrowser、SDK等方式删除文件。详情请参见[删除文件](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/删除文件.md)。
-   通过OSS控制台、命令行工具ossutil、图形化工具ossbrowser、SDK等方式上传同名文件到OSS，会导致OSS内已有文件被覆盖。
-   若您在生命周期规则中配置了定期删除文件的规则，OSS会根据生命周期的配置定期删除符合条件的文件。详情请参见[设置生命周期规则](/cn.zh-CN/控制台用户指南/存储空间管理/基础设置/设置生命周期规则.md)。
-   若您配置了跨区域复制规则，且选择的是**增/删/改 同步**，则对源存储空间（Bucket）进行文件修改或删除操作时，操作会同步到目的Bucket。详情请参见[设置跨区域复制](/cn.zh-CN/控制台用户指南/存储空间管理/冗余与容错/设置跨区域复制.md)。
-   没有正确的配置Bucket访问权限，导致文件被他人恶意删除或覆盖。访问权限相关说明请参见[访问控制概述](/cn.zh-CN/开发指南/数据安全/访问控制/访问控制概述.md)。

## 如何避免误删、误覆盖

-   开启版本控制

    开启版本控制功能后，误删除、误覆盖的文件会以历史版本的形式存储在Bucket中，您可以随时恢复历史版本。详情请参见[版本控制介绍](/cn.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)。

-   使用跨区域复制备份文件

    您可以使用跨区域复制的**增/改 同步**功能，将数据备份到不同地域的另一个Bucket中。详情请参见[设置跨区域复制](/cn.zh-CN/控制台用户指南/存储空间管理/冗余与容错/设置跨区域复制.md)。

-   使用定时备份功能定时备份文件

    您可以配置定时备份功能，将您的文件定时备份到混合云备份中。当您的文件丢失时，可第一时间恢复您的文件。详情请参见[定时备份](/cn.zh-CN/控制台用户指南/文件管理/定时备份.md)。

-   配置正确的访问权限

    为Bucket的访问者配置正确的访问权限，配置时注意遵循以下原则：

    -   不使用主账号访问OSS。
    -   读写分离，对于只需要读数据的业务，只使用具有读权限的子账号或STS临时凭证。
    -   开放给临时用户访问时，推荐使用STS的临时凭证来访问OSS。
    -   针对不同的业务，授予“够用且最小的范围”的OSS权限。
    -   妥善保管数据访问的凭据，如阿里云账号密码、RAM子账号访问凭据。
    权限配置详情请参见[访问控制概述](/cn.zh-CN/开发指南/数据安全/访问控制/访问控制概述.md)。


