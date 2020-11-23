# du

The du command is used to query the capacity of a bucket or the size of an object or a folder.

**Note:** The commands described in this topic apply to Linux. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.

## Command syntax

```
./ossutil du oss://bucket[/prefix] [--payer requester] [--all-versions]
```

## Examples

-   Query the size of a specific bucket

    Run the following command to query the size of objects of the current versions in bucket1:

    ```
    ./ossutil du oss://bucket1
    ```

    A similar command output is displayed:

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        6                       66057605
    ----------------------------------------------------------
    total object count: 6                           total object sum size: 66057605
    total part count:   0                           total part sum size:   0
    
    total du size(byte):66057605
    ```

    bucket1 contains six objects of the Standard storage class. The total size of the objects is 66,057,605 bytes.

-   Query the size of a specific folder

    Run the following command to query the sizes of objects of the current versions in the test folder of bucket1:

    ```
    ./ossutil du oss://bucket1/test/
    ```

    A similar command output is displayed:

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        3                       18325825
    ----------------------------------------------------------
    total object count: 3                           total object sum size: 18325825
    total part count:   0                           total part sum size:   0
    
    total du size(byte):18325825
    ```

    The test folder contains three objects of the Standard storage class. The total size of the objects is 18,325,825 bytes.

-   Query the size of a specific object

    Run the following command to query the size of the test.txt object in bucket1:

    ```
    ./ossutil du oss://bucket1/test.txt
    ```

    A similar command output is displayed:

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    IA               1                       8887851
    ----------------------------------------------------------
    total object count: 1                           total object sum size: 8887851
    total part count:   0                           total part sum size:   0
    
    total du size(byte):8887851
    ```

    The test.txt object is of the IA storage class and the size of the object is 8,887,851 bytes.

-   Query the size of all objects that have a specific prefix

    Run the following command to query the size of all objects whose names contain the test prefix in bucket1:

    ```
    ./ossutil du oss://bucket1/test
    ```

    A similar command output is displayed:

    ```
    storage class   object count            sum size(byte)
    ----------------------------------------------------------
    Standard        3                       18325825
    ----------------------------------------------------------
    total object count: 3                           total object sum size: 18325825
    total part count:   0                           total part sum size:   0
    
    total du size(byte):18325825
    ```

    bucket1 contains three objects whose names contain the prefix "test". The storage class of the objects is Standard and the total size of the objects is 18,325,825 bytes.

-   Query the size of a specific bucket, including the size of previous versions of objects in the bucket

    Run the following command to query the size of objects of all versions in bucket1:

    ```
    ./ossutil du oss://bucket1 --all-versions
    ```

    A similar command output is displayed:

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

    bucket1 contains 13 objects, of which 12 objects are of the Standard storage class and 1 is of the Archive storage class. The total size of the objects is 132,116,024 bytes.

    **Note:** The du command does not count the delete markers.


## Common options

The following table describes the options you can add to the du command.

|Option|Description|
|------|-----------|
|--loglevel|Specifies the log level. This option is empty by default, which indicates that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. Examples: `http://120.79.**.**:3128` and `socks5://120.79.**.**:1080`.|
|--proxy-user|Specifies the username of the proxy server. By default, this option is empty.|
|--proxy-pwd|Specifies the password for the proxy server. By default, this option is empty.|
|--payer|If pay-by-requester is enabled for the specified bucket, this option is required and must be set to `requester`.|
|--all-versions|Specifies all versions of an object.|

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

