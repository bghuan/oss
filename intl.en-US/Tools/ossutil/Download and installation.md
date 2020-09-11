# Download and installation

This topic describes how to download and install ossutil.

## Version and runtime environment

-   The current version: 1.6.19
-   Source code: [ossutil](https://github.com/aliyun/ossutil)
-   Runtime environment
    -   Windows/Linux/Mac
    -   Supported architectures: x86 \(32-bit and 64-bit\) and ARM \(32-bit and 64-bit\)

## Download URLs

-   [Linux x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutil32)
-   [Linux x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutil64)

    **Note:** When you copy the URLs to the wget command to download ossutil, delete the ? spm=xxxx section from the URLs.

-   [Windows x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutil32.zip)
-   [Windows x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutil64.zip)
-   [mac x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutilmac32)
-   [mac x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutilmac64)
-   [ARM 32bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutilarm32)
-   [ARM 64bit](http://gosspublic.alicdn.com/ossutil/1.6.19/ossutilarm64)

## Installation

Download the package based on your operating system and run the corresponding binary file.

-   Install ossutil in Linux \(the 64-bit Linux system is used as an example\)
    1.  Download the ossutil installation package.

        ```
        wget http://gosspublic.alicdn.com/ossutil/1.6.19/ossutil64                           
        ```

    2.  Modify the file execution permissions.

        ```
        chmod 755 ossutil64
        ```

    3.  Generate a configuration file based on the interactive processing.

        ```
        ./ossutil64 config
        Enter the name of the configuration file. The file name can contain a path. The default path is /home/user/.ossutilconfig. If you press Enter without specifying a different destination, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path. 
        If you do not specify the path of the configuration file, the default path is used. The default path is /home/user/.ossutilconfig. 
        The following parameters are ignored if you press Enter without configuring them. To obtain more information about the parameters, run the help config command.
        Enter the language: CH or EN. The default language is CH. This parameter takes effect after the config command is run. 
        Enter the endpoint: http://oss-cn-shenzhen.aliyuncs.com 
        Enter the AccessKey ID: yourAccessKeyID 
        Enter the AccessKey secret: yourAccessKeySecret
        Enter the STS token: 
        ```

        -   endpoint: specifies the domain name of the region to which the bucket belongs. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
        -   accessKeyID: For more information about how to view the AccessKey ID.
        -   accessKeySecret: For more information about how to view the AccessKey secret.
        -   stsToken: This option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave this parameter unspecified. For more information about how to generate an STS token, see [Temporary access credential](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).
        **Note:** For more information about the configuration file, see [config](/intl.en-US/Tools/ossutil/Common commands/config.md).

-   Install ossutil in Windows \(the 64-bit Windows system is used as an example\)
    1.  Download the ossutil installation package.
    2.  Decompress the package to a specified folder. Double-click and run theossutil.bat file.
    3.  Generate the configuration file. For more information about the parameters, see the configuration parameters described in the preceding Linux section.

        ```
        D:\ossutil>ossutil64.exe config
        ```

-   Install ossutil in macOS \(the 64-bit macOS system is used as an example\)
    1.  Download the ossutil installation package.

        ```
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.6.19/ossutilmac64
        ```

    2.  Modify the file execution permissions.

        ```
        chmod 755 ossutilmac64
        ```

    3.  Generate the configuration file. For more information about the parameters, see the configuration parameters described in the preceding Linux section.

        ```
        ./ossutilmac64 config
        ```


