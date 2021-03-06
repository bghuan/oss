# 修改文件读写权限 {#concept_h2y_r4l_vdb .concept}

OSS提供权限控制ACL（Access Control List），您可以在上传文件的时候设置相应的ACL权限控制，也可以在上传之后修改ACL。如果不设置ACL，默认值为私有。

OSS ACL提供存储空间级别和文件级别的权限访问控制，目前有三种访问权限：

-   **私有**：只有该存储空间的拥有者可以对该存储空间内的文件进行读写操作，其他人无法访问该存储空间内的文件。
    -   如果bucket的读写权限为私有，则在获取文件访问URL时需设置**链接有效时间**。
    -   URL签名的链接有效时间是基于NTP计算的。您可以将此链接给与任何访问者，访问者可以在有效时间内，通过此链接访问该文件。存储空间为私有权限时获得的地址是通过[在URL中包含签名](../../../../intl.zh-CN/API 参考/访问控制/在URL中包含签名.md#)生成的。
-   **公共读**：只有该存储空间的拥有者可以对该存储空间内的文件进行写操作，任何人（包括匿名访问者）可以对该存储空间中的文件进行读操作。
-   **公共读写**：任何人（包括匿名访问者）都可以对该存储空间中的文件进行读写操作，所有这些操作产生的费用由该存储空间的拥有者承担，请慎用该权限。

## 操作步骤 {#section_zjn_lql_vdb .section}

1.  进入[OSS 管理控制台](https://oss.console.aliyun.com/)界面。
2.  在左侧存储空间列表中，单击目标存储空间名称，打开该存储空间概览页面。
3.  单击**文件管理**页签。
4.  单击目标文件的文件名，打开该文件的预览页面。
5.  单击**设置读写权限**，修改该文件的读写权限。
    -   如果bucket的读写权限为**私有**，则在获取文件访问URL时需设置**链接有效时间**。
    -   在文件的预览页面的**签名**处输入**链接有效时间**，单位为秒。
6.  单击**确定** 。

