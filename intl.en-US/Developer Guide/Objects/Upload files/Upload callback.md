# Upload callback

After an object is uploaded, OSS can start a callback process for the application server. To request callback, you need only to send a request that contains relevant callback parameters to OSS.

**Note:**

-   For more information about how to use OSS operations to implement callbacks, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).
-   Operations that support callbacks include [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md), [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md), and [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

## Scenarios

Upload callback is used together with authorized third-party upload. When you upload an object to OSS, the client requests a callback to the server. After OSS completes the upload task of the client, OSS automatically sends an HTTP request for the callback to the application server to notify the server that the upload is completed. Then, the server performs operations such as modifying the database and responds to the callback request. After OSS receives the response, OSS returns the upload status to the client.

When OSS sends a POST callback request to the application server, OSS includes parameters in the POST request body to carry specific information. Such parameters are divided into two types: system-defined parameters such as the bucket name and the object name, and user-defined parameters. You can specify user-defined parameters based on the application logic when you send a request that contains callback parameters to OSS. You can use user-defined parameters to carry information relevant to the application logic such as the ID of the user who sends the request. For more information about how to use user-defined parameters, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).

You can properly use upload callback to simplify the client logic and save network resources. The following process describes how to implement upload callback:

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9295688951/p1064.jpg)

**Note:** Only simple upload \(by using the PutObject operation\), form upload \(by using the PostObject operation\), and multipart upload \(by using the CompleteMultipartUpload operation\) support upload callback.

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Upload callback.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Upload callback.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Upload objects/Upload callback.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Upload callback.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Upload objects/Upload callback.md)|

## References

-   [Upload callback]()
-   [Use PostObject on the web to upload objects](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Use PostObject on the web to upload objects.md)
-   [Set up upload callback for mobile apps](/intl.en-US/Best Practices/Application server/Set up upload callback for mobile apps.md)

