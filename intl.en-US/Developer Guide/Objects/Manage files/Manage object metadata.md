# Manage object metadata

Object metadata describes the attributes of objects uploaded to OSS. These attributes are classified into two types: HTTP standard attributes \(HTTP header fields\) and user metadata \(user-defined object metadata\).

The following section describes the HTTP standard attributes and user metadata:

-   HTTP standard attributes

    |Name|Description|
    |----|-----------|
    |Content-Type|The type of the object.|
    |Content-Encoding|The compression type of the object. Valid values:    -   gzip: uses the Lempel-Ziv coding \(LZ77\) compression algorithm and 32-bit CRC.
    -   compress: uses the Lempel-Ziv-Welch \(LZW\) compression algorithm.
    -   deflate: uses the zlib structure and deflate compression algorithm.
    -   identity: Data is not compressed. This is the default value.
    -   br: uses the Brotli encoding method. |
    |Content-Language|The language of the object.|
    |Content-Disposition|The format of the object. Valid values:    -   inline: The object is opened by using an application such as a browser.
    -   attachment: downloads an object to a local computer. You can use the additional `filename` to set the name of the object that can be saved locally. Example: `attachment; filename="example.jpg"`.
**Note:** When you access an object in OSS by using a browser, you can directly download the object by setting Content-Disposition to inline under the following conditions:

    -   The object is a web page object. You access the object without using the custom domain bound to the bucket.
    -   The object is an image and the bucket is created after September 23, 2019. You access the object without using the custom domain name bound to the bucket.
    -   The object is not supported for browsers to preview.
For more information, see the "Share an object" section in [Download objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md). |
    |Cache-Control|The cache settings of the object. Common settings:    -   no-cache: The cache cannot be used directly. It must be first verified by the server.
    -   no-store: No cache is used.
    -   public: The object can be cached on each node in the route in which the response is returned.
    -   private: The object can be cached only in the browser of the client.
    -   max-age=<seconds\>: the cache duration. Unit: seconds.
Example: `public, max-age=20` indicates that the object is cached for 20 seconds.|
    |Expires|The time the object expires in GMT. If `max-age=<seconds>` is set for **Cache-Control**, `max-age=<seconds>` takes precedence over Expires.|
    |Last-Modified|The time when the object is last modified.|
    |Content-Length|The size of the object.|

-   User Meta

    User metadata allows you to better describe objects. In OSS, all parameters prefixed with `x-oss-meta-` are considered as user metadata. Example: `x-oss-meta-location`. An object may have multiple similar parameters, but the total size of the user metadata cannot exceed 8 KB. User metadata is returned in the HTTP header of responses to GetObject or HeadObject requests.


## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object HTTP headers.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Manage Object Meta.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/Manage object metadata.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Manage object metadata.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Manage objects/Manage Object Meta.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Manage objects/Obtain the metadata of an object.md)|

## References

You can configure object metadata when you upload or copy objects. For more information, see:

-   [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md)
-   [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md)
-   [Append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md)
-   [Copy objects](/intl.en-US/Developer Guide/Objects/Manage files/Copy objects.md)

