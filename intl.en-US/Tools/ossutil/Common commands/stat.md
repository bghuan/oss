# stat

The stat command is used to query the description of buckets or objects. For example, you can run the stat command to view the object metadata that is set by running the set-meta command.

**Note:**

-   The commands described in this topic apply to Linux systems. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.
-   To run the stat command to view object metadata by using a RAM user, the RAM user must have permissions to perform the HeadObject, GetObjectAcl, and GetBucketInfo operations on the objects.

## Command syntax

```
./ossutil stat oss://bucket[/object] [--encoding-type url] [--payer requester] [-c file]
```

## Examples

-   Query the information about a bucket

    ```
     ./ossutil stat oss://bucket1
    ```

-   Query the information about a specified object

    ```
    ./ossutil stat oss://bucket1/object
    ```

-   Query the information about an object whose names contain special characters

    ossutil only supports URL encoding for object names. If an object name contains special characters, you can encode these special characters before you use the object name in the command. For example, you can run the following command to query the information about an object named 示例.txt:

    ```
    ./ossutil stat oss://bucket1/%E7%A4%BA%E4%BE%8B.txt --encoding-type url
    ```

    **Note:** Bucket names cannot be URL-encoded.

-   View a specified version of an object in a bucket that has versioning enabled

    ```
    ./ossutil stat oss://bucket1/test.jpg --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk
    ```

    To use the `--version-id` option, you must run the [ls --all-versions](/intl.en-US/Tools/ossutil/Common commands/ls.md) command to obtain all available versions of the object.

    **Note:** The `--version-id` option can be used only for objects in versioning-enabled buckets. For more information about the command that is used to enable versioning on a bucket, see [bucket-versioning](/intl.en-US/Tools/ossutil/Common commands/bucket-versioning.md).


## Common options

The following table describes the options you can add to the stat command.

|Option|Description|
|------|-----------|
|--encoding-type|Specifies the method used to encode the object name. If this option is specified, the value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--loglevel|Specifies the log level. The default value is null, which indicates that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|--version-id|Specifies the version ID of an object in a versioned bucket.|
|--payer|Specifies the payer of the request. To enable pay-by-requester, set this option to requester.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. Examples: http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username of the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password for the proxy server. The default value is null.|

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

