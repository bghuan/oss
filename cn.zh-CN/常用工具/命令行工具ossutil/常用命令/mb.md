# mb

mb命令用于创建存储空间（Bucket）。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil mb oss://bucketname [--acl=ACL][--storage-class sc][-c file][--redundancy-type type]
```

## 使用示例

-   创建Bucket

    ```
    ./ossutil mb oss://bucket1
    ```

    **说明：** Bucket名称唯一，若创建的Bucket名称已存在，会出现Bucket已存在的报错信息。

-   创建Bucket时指定访问权限

    ```
    ./ossutil mb oss://bucket1 --acl public-read-write
    ```

    --acl：用来指定存储空间访问权限，默认为私有（private）。

    -   private：私有
    -   public-read：公共读
    -   public-read-write：公共读写
    ACL详情请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。

-   创建Bucket时指定存储类型

    ```
    ./ossutil mb oss://bucket1 --storage-class IA
    ```

    --storage-class：用来指定Bucket的默认存储类型。默认存储类型为标准存储类型（Standard）。

    -   Standard：标准存储
    -   IA：低频访问
    -   Archive：归档存储
    -   ColdArchive：冷归档存储
    存储类型详情请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。

-   在指定地域创建Bucket

    ```
    ./ossutil mb oss://bucket1 -e oss-cn-beijing.aliyuncs.com
    ```

-   使用指定的配置文件创建Bucket

    ```
    ./ossutil mb oss://bucket1 -c your_config_file_path
    ```

-   创建Bucket并开启同城冗余存储

    ```
    ./ossutil mb oss://bucket1 --redundancy-type ZRS
    ```

    --redundancy-type：设置Bucket的数据冗余类型，可选值为LRS（本地冗余存储）和ZRS（同城冗余存储），默认为LRS。

    **说明：** 目前仅在华南 1（深圳）、华北 2（北京）、华东 2（上海）地域支持同城冗余存储，如果您在创建Bucket时设置该参数的值为ZRS，请注意您的Endpoint配置。关于同城冗余存储的更多信息请参见[同城冗余存储](/cn.zh-CN/开发指南/数据安全/数据容灾/同城冗余存储.md)。


## 常用选项

您可以在使用mb命令时，指定如下选项来指定Bucket属性：

|选项名称|描述|
|----|--|
|--acl|用来指定存储空间访问权限，默认为私有（private）。取值范围： -   private：私有
-   public-read：公共读
-   public-read-write：公共读写

ACL详情请参见[读写权限ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。|
|--storage-class|用来指定Bucket的默认存储类型。默认存储类型为标准存储类型（Standard）。 取值范围： -   Standard：标准存储
-   IA：低频访问
-   Archive：归档存储
-   ColdArchive：冷归档存储

存储类型详情请参见[存储类型介绍](/cn.zh-CN/开发指南/存储类型/存储类型介绍.md)。 |
|-c，--config-file|使用指定的config文件来创建Bucket。未附加该参数则表示使用默认配置文件创建Bucket。config配置请参见[config](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/config.md)。|
|-e，--endpoint|用来指定Bucket的地域（Region）。未附加该参数则表示使用默认配置文件中的Endpoint来指定Bucket的地域。|
|-L，--language|设置ossutil工具的语言，默认值：CH，取值范围：CH/EN，若设置成“CH”，请确保您的系统编码为UTF-8。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1-500。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|
|--redundancy-type|设置Bucket的数据冗余类型，可选值为LRS（本地冗余存储）和ZRS（同城冗余存储），默认为LRS。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

