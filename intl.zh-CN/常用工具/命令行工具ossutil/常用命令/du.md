# du

du命令用于获取指定存储空间（Bucket）、文件目录或文件（Object）的大小。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil du oss://bucket[/prefix] [--payer requester] [--all-versions]
```

## 使用示例

-   查询指定Bucket大小

    以下命令用于查询bucket1内所有当前版本文件的大小：

    ```
    ./ossutil du oss://bucket1
    ```

    输出结果如下：

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        6                       66057605
    ----------------------------------------------------------
    total object count: 6                           total object sum size: 66057605
    total part count:   0                           total part sum size:   0
    
    total du size(byte):66057605
    ```

    获取到bucket1中共包含6个文件，存储类型为Standard（标准存储），文件总大小为66057605 Byte。

-   查询指定文件目录大小

    以下命令用于查询bucket1中test目录内所有当前版本文件的大小：

    ```
    ./ossutil du oss://bucket1/test/
    ```

    输出结果如下：

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        3                       18325825
    ----------------------------------------------------------
    total object count: 3                           total object sum size: 18325825
    total part count:   0                           total part sum size:   0
    
    total du size(byte):18325825
    ```

    获取到test目录中共包含3个文件，存储类型为Standard，文件总大小为18325825 Byte。

-   查询指定Object大小

    以下命令用于查询bucket1中test.txt文件的大小：

    ```
    ./ossutil du oss://bucket1/test.txt
    ```

    输出结果如下：

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    IA               1                       8887851
    ----------------------------------------------------------
    total object count: 1                           total object sum size: 8887851
    total part count:   0                           total part sum size:   0
    
    total du size(byte):8887851
    ```

    获取到test.txt的存储类型为IA（低频访问存储），文件大小为8887851 Byte。

-   查询指定前缀的所有Object大小

    以下命令用于查询bucket1中所有前缀为test文件的大小：

    ```
    ./ossutil du oss://bucket1/test
    ```

    输出结果如下：

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        3                       18325825
    ----------------------------------------------------------
    total object count: 3                           total object sum size: 18325825
    total part count:   0                           total part sum size:   0
    
    total du size(byte):18325825
    ```

    获取到前缀为test的文件共3个，存储类型为Standard，文件总大小为18325825 Byte。

-   查询指定Bucket大小，包括历史版本的文件

    以下命令用于查询bucket1内所有版本文件的大小：

    ```
    ./ossutil du oss://bucket1 --all-versions
    ```

    输出结果如下：

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        12                       132115210
    Archive         1                        814
    ----------------------------------------------------------
    total object count: 13                          total object sum size: 132116024
    total part count:   0                           total part sum size:   0
    
    total du size(byte):132116024
    ```

    获取到bucket1中共包含13个文件，其中12个文件的存储类型为Standard，1个为Archive（归档存储），文件总大小为132116024 Byte。

    **说明：** du命令不会统计删除标记。


## 常用选项

您可以在使用du命令时，附加如下选项：

|选项名称|描述|
|----|--|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括HTTP请求和响应信息）。 |
|--proxy-host|网络代理服务器的URL地址，支持HTTP、HTTPS、SOCKS5。例如`http://120.79.**.**:3128`、`socks5://120.79.**.**:1080`。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|
|--payer|若目标Bucket已开启请求者付费模式，需增加此项，并将该值设置为`requester`。|
|--all-versions|表示Object所有版本。|

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

