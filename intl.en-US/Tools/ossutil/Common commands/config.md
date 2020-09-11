# config

The config command is used to create a configuration file to store OSS access information. You can add the -c option to provide access information when accessing OSS.

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
    Enter the name of the configuration file. The file name can contain a path, and the default path is /home/user/.ossutilconfig. If you press Enter without specifying a different destination, the file will be generated in the default path. If you want to generate the file in another path, set the --config-file option to the path. 
    If the path of the configuration file is not entered, the default configuration file /home/user/.ossutilconfig is used. 
    The following parameters are ignored if you press Enter without configuring them. To obtain more information about the parameters, run the help config command. 
    Enter endpoint:http://oss-cn-shenzhen.aliyuncs.com 
    Enter the AccessKey ID: your AccessKey ID 
    Enter the AccessKey secret: your AccessKey secret
    Enter the STS token: 
    ```

    -   endpoint: specifies the domain name of the region to which the bucket belongs. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
    -   accessKeyID: For more information about how to view the AccessKey ID.
    -   accessKeySecret: For more information about how to view the AccessKey secret.
    -   stsToken: This option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave this parameter unspecified. For more information about how to generate an STS token, see [Temporary access credential](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).
-   Generate a configuration file in non-interactive mode

    ```
    ./ossutil config -e oss-cn-beijing.aliyuncs.com -i LTAIbZcdVCmQ**** -k D26oqKBudxDRBg8Wuh2EWDBrM0****  -L CH -c /myconfig
    ```

    If you run the config command with options other than --language and --config-file, the non-interactive mode is used. All configuration items are specified using options.


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

-   Bucket-Endpoint: specifies an individual endpoint for each specified bucket. If you configure the Bucket-Endpoint option, ossutil searches for the specified endpoint when you perform operations on the bucket. If the specified endpoint exists, ossutil manages the bucket through that endpoint. Otherwise, ossutil manages the bucket through the endpoint specified in the Credentials option.
-   Bucket-Cname:Bucket-Cname you to configure an individual CNAME \(CDN domain\) for each specified Bucket. This setting takes precedence over the Bucket-Endpoint setting in the configuration file and the endpoint setting in the Credentials option. For more information about canonical domain names, see the "Configure a canonical name \(CNAME\) record" section in [Overview](/intl.en-US/Quick Start/Overview.md).
-   AkService: This option is not added by default. This option is required if you need to use a RAM role bound to an ECS instance to perform operations on OSS. When you configure this option, you only need to set EcsRamRoleTesting to the RAM role bound to the ECS instance. After configuration, you do not need to configure the accessKeyID, accessKeySecret, and stsToken parameters. If accessKeyID is configured, the configurations of the AkService do not take effect, and you must use the configured accessKeyID, accessKeySecret, and stsToken parameters for identity authentication. For more information about how to bind a RAM role to an ECS instance, see use RAM roles in the console. For more information about how to bind a RAM role to an ECS instance, see [Bind an instance RAM role](/intl.en-US/Security/Instance RAM roles/Bind an instance RAM role.md).

**Note:**

-   In the new version of ossutil, the Bucket-Endpoint and Bucket-Cname options do not need to be specified in interactive mode. However, the two options still take effect in the configuration file. You can specify an endpoint or accelerated domain name for each bucket in the configuration file.
-   ossutil can specify endpoints for multiple regions. The priorities of all of the endpoint configurations are as follows: endpoint specified by the `--endpoint` option in commands \> endpoint specified in Bucket-Cname \> endpoint specified in Bucket-Endpoint \> endpoint specified in Credentials. If you specify the `--endpoint` \(abbreviated as `-e`\) option when running a command, the value of the `--endpoint` option takes precedence. If this option is not specified, ossutil searches for the endpoints specified in Bucket-Cname, Bucket-Endpoint, and Credentials in the configuration file. The endpoint with the highest priority is used.

## Common options

The following table describes the options you can add to the config command to generate the specified configuration items.

|Option|Description|
|------|-----------|
|-e, --endpoint|Specifies the endpoint in the \[Credentials\] section of the configuration file.|
|-i, --access-key-id|Specifies the AccessKey ID in the \[Credentials\] section of the configuration file.|
|-k, --access-key-secret|Specifies the AccessKey secret in the \[Credentials\] section of the configuration file.|
|-t, --sts-token|Optional. This option specifies the STS token in the \[Credentials\] section of the configuration file.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. The default value is the ossutil\_output directory in the current directory.|
|-L, --language|Specifies the language ossutil uses. Valid values: CH and EN. Default value: CH. To set this option to CH, make sure that your system supports UTF-8 encoding.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detail logs and their corresponding HTTP request and response information. |

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

