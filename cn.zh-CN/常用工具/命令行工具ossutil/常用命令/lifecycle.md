# lifecycle

lifecycle命令用于添加、修改、查询、删除生命周期规则配置。

**说明：**

-   本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。
-   生命周期规则介绍请参见[生命周期规则介绍](/cn.zh-CN/开发指南/对象/文件（Object）/文件生命周期/生命周期规则介绍.md)。

## 命令格式

-   添加/修改生命周期规则配置

    ```
    ./ossutil lifecycle --method put oss://bucket local_xml_file [options]
    ```

    ossutil从配置文件local\_xml\_file中读取生命周期规则配置，并在Bucket中添加对应规则。若添加的生命周期规则ID已经存在，则会覆盖已有ID的生命周期规则配置。

    配置文件是一个xml格式的文件，可以配置文件过期后转为低频访问、归档存储、冷归档存储，或直接删除文件。您可以选择部分规则进行配置，完整配置举例如下：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>RuleID</ID>
        <Prefix>Prefix</Prefix>
        <Status>Status</Status>
        <Expiration>
          <Days>Days</Days>
        </Expiration>
        <Transition>
          <Days>Days</Days>
          <StorageClass>StorageClass</StorageClass>
        </Transition>
        <AbortMultipartUpload>
          <Days>Days</Days>
        </AbortMultipartUpload>
      </Rule>
    </LifecycleConfiguration>
    ```

    配置文件内容介绍请参见[生命周期配置元素](/cn.zh-CN/开发指南/对象/文件（Object）/文件生命周期/生命周期配置元素.md) 。

-   获取生命周期规则配置

    ```
    ossutil lifecycle --method get oss://bucket [local_file]
    ```

    local\_file为文件路径参数。若填写，则将生命周期规则配置保存为本地文件；若置空，则将生命周期规则配置输出到屏幕上。

-   删除生命周期规则配置

    ```
    ./ossuitl lifecycle --method delete oss://bucket
    ```


## 使用示例

-   添加生命周期规则，将已上传10天，且前缀为test/的文件转为低频访问类型

    ```
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    lifecycle.xml文件内容为：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>1</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>10</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
     </Rule>
    </LifecycleConfiguration>
    ```

-   添加生命周期规则，将文件最后修改日期2019年1月1日前，且前缀为test/的文件删除

    ```
    ./ossutil lifecycle --method put oss://bucket1  /file/lifecycle.xml
    ```

    lifecycle.xml文件内容为：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>2</ID>
        <Prefix>test/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2019-01-01T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
     </Rule>
    </LifecycleConfiguration>
    ```

-   获取生命周期规则配置

    ```
    ./ossutil lifecycle --method get oss://bucket1  /file/lifecycle.xml
    ```

-   删除生命周期规则配置

    ```
    ./ossutil lifecycle --method delete oss://bucket1  
    ```


## 常用选项

您可以在使用lifecycle命令时附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

