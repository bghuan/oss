# restore

restore命令用于恢复冷冻状态的对象（Object）为可读状态。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil restore cloud_url [--encoding-type url] [--payer requester] [-r] [-f] [--output-dir=odir] [-c file] [local_xml_file]
```

## 使用示例

-   解冻单个归档类型Object

    ```
    ./ossutil restore oss://bucket/object
    ```

-   解冻指定前缀的归档类型Object

    ```
    ./ossutil restore oss://bucket/path/ -r                         
    ```

    **说明：**

    -   解冻归档类型Object需要1分钟时间，解冻中无法读取文件。
    -   解冻状态默认持续 1 天，对解冻状态的文件调用restore命令，会将文件的解冻状态延长1天，最多可以延长到7天，之后文件又回到初始时的冷冻状态。
-   解冻冷归档类型的Object

    ```
    ./ossutil restore oss://bucket/object ./config.xml
    ```

    config.xml是本地XML格式文件，内容包含冷归档的解冻参数。示例：

    ```
    <RestoreRequest>
        <Days>3</Days>
        <JobParameters>
            <Tier>Bulk</Tier>
        </JobParameters>
    </RestoreRequest>
    ```

    -   Days：设置冷归档文件保持解冻状态的时间，单位为天，取值范围1~7。
    -   Tier：设置冷归档文件的解冻模式，可选值为Expedited、Standard、Bulk。

        -   Expedited（高优先级）：1小时内完成解冻。
        -   Standard（标准）：2~5小时完成解冻。
        -   Bulk（批量）：5~12小时完成解冻。
        根据解冻文件的大小，实际解冻时间可能会有变化，请以实际解冻时间为准。

-   在已开启版本控制的Bucket内解冻指定版本的归档类型Object

    ```
    ./ossutil restore oss://bucket1/test.jpg --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    ```

    在使用--version-id选项前，需使用[ls --all-versions](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls.md)命令获取文件的versionid。

    **说明：** --version-id选项仅支持在已开启版本控制的Bucket内使用。开启Bucket版本控制命令请参见[bucket-versioning](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/bucket-versioning.md)。


## 常用选项

您可以在使用restore命令时附加如下选项：

|选项名称|描述|
|----|--|
|-r，--recursive|递归进行操作。当指定该选项时，命令会对Bucket下所有符合条件的Object进行操作，否则只对指定的单个Object进行操作。|
|-f，--force|强制操作，不进行询问提示。|
|-j，--jobs|多文件操作时的并发任务数，默认值：3，取值范围：1~10000。|
|--encoding-type|输入或者输出的Object名的编码方式，目前只支持url编码，即指定该选项时，取值为url。如果不指定该选项，则表示Object名未经过编码。Bucket名不支持url编码。|
|--output-dir=|指定输出文件所在的目录，输出文件目前包含：cp命令批量拷贝文件出错时所产生的report文件（关于report文件更多信息，请参考cp命令帮助）。默认值为：当前目录下的ossutil\_output目录。|
|--version-id|操作指定版本的Object，仅支持在已开启版本控制的Bucket内使用。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--retry-times|当错误发生时的重试次数，默认值：10，取值范围：1~500。|
|--payer|请求的支付方式，如果为请求者付费模式，需将该值设置为requester。|
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

