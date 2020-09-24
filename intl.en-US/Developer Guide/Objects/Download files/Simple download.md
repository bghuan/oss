# Simple download

By using simple download, you can send an HTTP GET request to download an uploaded object by calling the GetObject operation of OSS.

**Note:**

-   For more information about GetObject, see [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md).
-   For more information about the rules for generating object URLs, see [OSS request process](/intl.en-US/Developer Guide/Data security/Signature/OSS request process.md).
-   For more information about how to use a custom domain to access an object, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Download objects/Download to local files.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Download objects/Download an object to a local file.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Download objects/Download to your local file.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Download objects/Download to your local file.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Download objects/Download an object to a local file.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Download objects/Download to local files.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Download objects/Simple download.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Download objects/Overview.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/Download objects.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Download objects.md)|

## Download security and authorization

-   To prevent unauthorized third-party users from downloading data from your bucket, OSS provides bucket- and object-level access control. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).
-   For more information about how to authorize a third-party user to download objects from a private bucket, see [Authorized third-party users to download objects](/intl.en-US/Developer Guide/Objects/Download files/Authorized third-party users to download objects.md).

## References

For more information about how to use resumable download, see [Resumable download](/intl.en-US/Developer Guide/Objects/Download files/Resumable download.md).

