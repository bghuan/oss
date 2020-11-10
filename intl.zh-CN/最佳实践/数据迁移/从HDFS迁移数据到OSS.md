# 从HDFS迁移数据到OSS

本文介绍如何使用阿里云Jindo DistCp从HDFS迁移数据到OSS。

在传统大数据领域，HDFS经常作为大规模数据的底层存储。在进行数据迁移、数据拷贝的场景中，最常用的是Hadoop自带的DistCp工具。但是该工具不能很好利用对象存储OSS的特性，导致效率低下并且不能保证数据一致性。此外，该工具提供的功能选项也比较简单，不能很好的满足用户的需求。

阿里云Jindo DistCp（分布式文件拷贝工具）是用于大规模集群内部或集群之间拷贝文件的工具。它使用MapReduce实现文件分发，错误处理和恢复，把文件和目录的列表作为map/reduce任务的输入，每个任务会完成源列表中部分文件的拷贝。全量支持HDFS之间、HDFS与OSS之间、以及OSS之间的数据拷贝场景，提供多种个性化拷贝参数和多种拷贝策略。

相对于Hadoop DistCp，使用阿里云Jindo DistCp从HDFS迁移数据到OSS具有以下优势：

-   效率高，在测试场景中最高可达到1.59倍的加速。
-   基本功能丰富，提供多种拷贝方式和场景优化策略。
-   深度结合OSS，对文件提供直接归档和低频、压缩等操作。
-   实现No-Rename拷贝，保证数据一致性。
-   场景全面，可完全替代Hadoop DistCp，目前支持Hadoop2.7+和Hadoop3.x。

## 前提条件

-   如果您使用的是自建ECS集群，需要具备Hadoop2.7+或Hadoop3.x环境以及进行MapReduce作业的能力。
-   如果您使用的是阿里云E-MapReduce：
    -   对于EMR3.28.0/bigboot2.7.0及以上的版本，可以通过Shell命令的方式使用Jindo DistCp。详情请参见[t1903713.dita\#task\_2526637](/intl.zh-CN/SmartData/SmartData基础使用（EMR-3.28.x版本）/Jindo DistCp使用说明.md)。
    -   对于EMR3.28.0以下的版本，可能会存在一定的兼容性问题，您可以通过提交工单申请处理。

## 步骤1：下载JAR包

-   [下载针对Hadoop 2.7+的JAR包](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp/Hadoop2.7%2BS3/jindo-distcp-2.7.3.jar)
-   [下载针对Hadoop 3.x的JAR包](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp/Hadoop3.x%2BS3/jindo-distcp-2.7.3.jar)

## 步骤2：配置OSS的访问密钥AccessKey

您可以通过以下任意方式配置AccessKey：

-   在命令中指定--key、--secret、--endPoint参数选项来指定AccessKey。示例命令如下：

    ```
    hadoop jar jindo-distcp-2.7.3.jar --src /data/incoming/example_file --dest oss://example_folder/example_file --key yourAccessKeyId --secret yourAccessKeySecret --endPoint oss-cn-hangzhou.aliyuncs.com
    ```

    -   --src：指定源文件路径，该示例为`/data/incoming/example_file`。
    -   --dest：指定目标文件路径，该示例为`oss://example_folder/example_file`。
    -   --key：您的AccessKey ID。关于AccessKey ID的介绍请参见[创建AccessKey]()。
    -   --secret：您的AccessKey Secret。关于AccessKey Secret的介绍请参见[创建AccessKey]()。
    -   --endPoint：目标文件的Bucket所在地域（Region）对应的访问域名（Endpoint），该示例为`oss-cn-hangzhou.aliyuncs.com`。关于OSS支持的地域和对应的访问域名列表信息，请参见[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。
-   将OSS的--key、--secret、--endPoint预先配置在Hadoop的core-site.xml文件里。配置如下：

    ```
    <configuration>
        <property>
            <name>fs.jfs.cache.oss-accessKeyId</name>
            <value>yourAccessKeyId</value>
        </property>
        <property>
            <name>fs.jfs.cache.oss-accessKeySecret</name>
            <value>yourAccessKeySecret</value>
        </property>
        <property>
            <name>fs.jfs.cache.oss-endpoint</name>
            <value>Endpoint</value>
        </property>
    </configuration>
    ```

-   配置免密功能，避免明文保存AccessKey，提高安全性。详情请参见[t1872858.dita\#task\_2435437](/intl.zh-CN/SmartData/SmartData基础使用（EMR-3.27.0之前版本）/使用JindoFS SDK免密功能.md)。

## 步骤3：设置相关参数

Jindo DistCp提供多种实用功能及其对应的参数选择。参数含义及其示例如下表所示。详细的使用方法和完整的命令示例请参见[Jindo DistCp使用指南](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master-2.x/docs/jindo_distcp_how_to.md?spm=a2c6h.12873639.0.0.700f97aeCrmwQP&file=jindo_distcp_how_to.md)。

|参数|是否必须|含义|示例|
|--|----|--|--|
|--src|是|指定拷贝的源路径。|--src oss://exampleBucket/sourceDir|
|--dest|是|指定拷贝的目标路径|--dest oss://exampleBucket/destDir|
|--parallelism|否|指定拷贝的任务并行度，可根据集群资源调节。|--parallelism 10|
|--policy|否|指定拷贝到OSS后的文件类型。取值：-   archive（归档）
-   ia（低频）

|--policy archive|
|--srcPattern|否|指定正则表达式来选择或者过滤需要拷贝的文件。您可以编写自定义的正则表达式来完成过滤操作，正则表达式必须为全路径正则匹配。|拷贝以`.log`结尾的文件：--srcPattern .\*\\.log |
|--deleteOnSuccess|否|指定是否在拷贝完成后删除源路径下的文件。|--deleteOnSuccess|
|--outputCodec|否|指定拷贝文件按何种方式压缩。当前版本支持编解码器gzip、gz、lzo、lzop和snappy，以及关键字none和keep，含义如下：-   none：保存为未压缩的文件。如果文件已压缩，则Jindo DistCp会将其解压缩。
-   keep（默认）：不更改文件压缩形态，按原样复制。

|--outputCodec gzip|
|srcPrefixesFile|否|指定需要拷贝的文件列表，列表里文件以src路径作为前缀。|--srcPrefixesFile file:///opt/folders.txt|
|--outputManifest|否|指定在dest目录下生成一个gzip压缩的文件，记录已完成拷贝的文件信息。|--outputManifest=manifest-2020-04-17.gz|
|--requirePreviousManifest|否|指定本次拷贝是否需要读取之前已拷贝的文件信息。取值：-   false：不读取，拷贝全量数据。
-   true：读取，仅拷贝增量数据。

|--requirePreviousManifest=false|
|--previousManifest|否|指定本次拷贝需要读取之前已拷贝文件的信息，完成增量更新。|--previousManifest=oss://exampleBucket/manifest-2020-04-16.gz|
|--copyFromManifest|否|从已完成的Manifest文件中进行拷贝，通常和--previousManifest配合使用。|--previousManifest oss://exampleBucket/manifest-2020-04-16.gz --copyFromManifest|
|--groupBy|否|指定正则表达式将符合规则的文件进行聚合，和targetSize选项配合使用。|--groupBy='.\*/\(\[a-z\]+\).\*.txt'|
|--targetSize|否|指定聚合后的文件大小阈值，单位为MB。|--targetSize=10|
|--enableBalancePlan|否|执行策略，适用于数据量差异不大的场景。|--enableBalancePlan|
|--enableDynamicPlan|否|执行策略，适用于数据量差异较大的场景，例如大文件和小文件混合的场景。|--enableDynamicPlan|
|--enableTransaction|否|执行策略，保证Job级别的一致性，默认是Task级别|--enableTransaction|
|--diff|否|对比策略，查看本次拷贝是否完成全部文件拷贝，未完成会生成文件列表。|--diff|
|--key|否|指定OSS访问的AccessKey ID。|--key yourAccessKeyId|
|--secret|否|指定OSS访问的AccessKey Secret。|--secret yourAccessKeySecret|
|--endPoint|否|指定OSS访问的地域信息。|--endPoint oss-cn-hangzhou.aliyuncs.com|
|--cleanUpPending|否|清理OSS残留文件，这可能会花费一定的时间。|--cleanUpPending|

