# config

The config command is used to create a configuration file to store OSS access information. You can add the -c option to other commands so that ossutil can use the specified configurations to access OSS buckets.

**Note:** The commands described in this topic apply to Linux. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.

## Command syntax

```
./ossutil config [-e endpoint] [-i id] [-k key] [-t token] [-L language] [--output-dir outdir] [-c file]
```

**Note:** This command can be used in both interactive and non-interactive modes. We recommend that you run the command in interactive mode to ensure security.

## Examples

-   Generate a configuration file in interactive mode

    ```
    ./ossutil config
    Enter the name of the configuration file. The file name can contain a path. The default path is /home/user/.ossutilconfig. If you press the Enter key without specifying a different destination, the file is generated in the default path. If you want to generate the file in another path,
    set the --config-file option to the path. 
    If you do not specify the path of the configuration file, the default path is used. The default path is /home/user/.ossutilconfig. 
    The following parameters are ignored if you press the Enter key without configuring them. For more information about the parameters, run the help config command. 
    Enter the endpoint: https://oss-cn-shenzhen.aliyuncs.com 
    Enter the AccessKey ID: yourAccessKeyID 
    Enter the AccessKey secret: yourAccessKeySecret
    Enter the STS token: 
    ```

    -   endpoint: specifies the domain name of the region to which the bucket belongs. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). You can also add `http://` or `https://` to specify the protocol that ossutil uses to access OSS. The default protocol is HTTP.
    -   accessKeyID: For more information about how to view the AccessKey ID, see [Create an AccessKey pair]().
    -   accessKeySecret: For more information about how to view the AccessKey secret, see [Create an AccessKey pair]().
    -   stsToken: This option is required only when you use a temporary STS token to access OSS buckets. Otherwise, you can leave this parameter empty. For more information about how to generate an STS token, see [Authorized third-party upload](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).
-   Generate a configuration file in non-interactive mode

    ```
    ./ossutil config -e oss-cn-beijing.aliyuncs.com -i LTAIbZcdVCmQ**** -k D26oqKBudxDRBg8Wuh2EWDBrM0****  -L CH -c /myconfig
    ```

    If you specify options besides --language and --config-file when you run the config command, non-interactive mode is used. All configuration items are specified by using options.


## Configuration file format

You can modify OSS access information in the generated configuration file. The configuration file is in the following format:

```
[Credentials]
        language = CH
        endpoint = oss.aliyuncs.com
        accessKeyID = your_key_id
        accessKeySecret = your_key_secret
        stsToken = your_sts_token
        outputDir = your_output_dir
[Bucket-Endpoint]
        bucket1 = endpoint1
        bucket2 = endpoint2
        ...
[Bucket-Cname]
        bucket1 = cname1
        bucket2 = cname2
        ...
[AkService]
        ecsAk=http://100.100.100.200/latest/meta-data/Ram/security-credentials/EcsRamRoleTesting
```

-   Bucket-Endpoint: specifies an individual endpoint for each specified bucket. If you configure the Bucket-Endpoint option, ossutil searches for the specified endpoint when you perform operations on the bucket. If the specified endpoint exists, ossutil manages the bucket by using that endpoint. Otherwise, ossutil manages the bucket by using the endpoint specified in the Credentials option.
-   Bucket-Cname: specifies an individual CNAME for each specified bucket. This setting takes precedence over the Bucket-Endpoint setting in the configuration file and the endpoint setting in the Credentials option. For more information about CNAME, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
-   AkService: By default, this option is not added. This option is required if you need to use a RAM role bound to an ECS instance to perform operations on OSS. When you configure this option, you only need to set EcsRamRoleTesting to the RAM role bound to the ECS instance. After you configure this option, you do not need to configure the accessKeyID, accessKeySecret, or stsToken parameters. If accessKeyID is configured, the configurations of AkService do not take effect, and you must use the configured accessKeyID, accessKeySecret, and stsToken parameters for authentication. For more information about how to bind a RAM role to an ECS instance, see [Bind an instance RAM role](/intl.en-US/Security/Instance RAM roles/Bind an instance RAM role.md).

**Note:**

-   In the new version of ossutil, the Bucket-Endpoint and Bucket-Cname options do not need to be specified in interactive mode. However, the two options still take effect in the configuration file. You can specify an endpoint or a CNAME for each bucket in the configuration file.
-   ossutil can specify endpoints for multiple regions. The endpoint configurations take effect in the following order: endpoint specified by the `--endpoint` option in commands \> endpoint specified in Bucket-Cname \> endpoint specified in Bucket-Endpoint \> endpoint specified in Credentials.

## Common options

The following table describes the options that you can add to the config command to generate the specified configuration items.

|Option|Description|
|------|-----------|
|-e,--endpoint|Specifies the endpoint in the \[Credentials\] section of the configuration file.|
|-i, --access-key-id|Specifies the AccessKey ID in the \[Credentials\] section of the configuration file.|
|-k, --access-key-secret|Specifies the AccessKey secret in the \[Credentials\] section of the configuration file.|
|-t, --sts-token|Optional. This option specifies the STS token in the \[Credentials\] section of the configuration file.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple files. The default value is the ossutil\_output directory in the current directory.|
|-L, --language|Specifies the language that ossutil uses. Valid values: CH and EN. Default value: CH. If you plan to set this option to CH, make sure that your system supports UTF-8 encoding.|
|--loglevel|Specifies the log level. This option is empty by default, which indicates that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

