# IMG persistence

By default, Image Processing \(IMG\) does not save processed images. You must add the saveas parameter to an IMG request. You can save the processed image as an object to a specified bucket. You can specify the name for the object. Then, you can access the processed image in the specified bucket.

You can use the object URL, SDK, or API operation to process images, and then save the processed images in OSS. For more information about IMG operations, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

## Object URL

Images that are processed by using object URLs cannot be directly saved to the specified bucket. You can save processed images to your local computer, and upload them to the specified bucket.

## SDK

When you use SDKs to process images, you can use the ImgSaveAs operation to save the images to the bucket where the source images are stored. The following code provides an example on how to use OSS SDK for Java to process images:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String sourceImage = "<yourSourceImageName>";
// Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
try {
    // Resize the image to a height and width of 100 pixels.
    StringBuilder sbStyle = new StringBuilder();
    Formatter styleFormatter = new Formatter(sbStyle);
    String styleType = "image/resize,m_fixed,w_100,h_100";
    String targetImage = "<targetImageName>";
    styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
            BinaryUtil.toBase64String(targetImage.getBytes()),
            BinaryUtil.toBase64String(bucketName.getBytes()));
    System.out.println(sbStyle.toString());
    ProcessObjectRequest request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
    GenericResult processResult = ossClient.processObject(request);
    String json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
    processResult.getResponse().getContent().close();
    System.out.println(json);
} catch (Exception e) {
    e.printStackTrace();
}
// Shut down the OSSClient instance.
ossClient.shutdown();
```

For more information about SDK demos for other programming languages, see the following topics:

-   [Java SDK](/intl.en-US/SDK Reference/Java/Image processing.md)
-   [Python SDK](/intl.en-US/SDK Reference/Python/Image processing.md)
-   [Go SDK](/intl.en-US/SDK Reference/Go/Image processing.md)
-   [C++ SDK](/intl.en-US/SDK Reference/C++/IMG.md)
-   [Android SDK](/intl.en-US/SDK Reference/Android/Image processing.md)
-   [iOS SDK](/intl.en-US/SDK Reference/iOS/IMG.md)
-   [Node.js SDK](/intl.en-US/SDK Reference/Node. js/IMG.md)

## API

You can call the PostObject operation to call IMG, and add the saveas parameter to an IMG request to save the processed image as an object in OSS. Parameters that follow x-oss-process are used the way you use queryString to call IMG functions.

The following table lists the parameters of saveas.

|Parameter|Description|
|---------|-----------|
|o|The name of the requested object. The parameter must be encoded in URL Safe Base64. For more information, see [Watermark encoding](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Add watermarks.md).|
|b|The name of the requested bucket. This parameter must be encoded in URL Safe Base64. By default, the processed image is saved to the current bucket if this parameter is not specified.|

Example: Resize the requested image and save it to the bucket named test. The object name is test.jpg.

```
POST /ObjectName? x-oss-process HTTP/1.1
Host: oss-example.oss.aliyuncs.com
Content-Length: 247
Date: Fri, 04 May 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT****=

x-oss-process=image/resize,w_100|sys/saveas,o_dGVzdC5qcGc,b_dGVzdA
```

If you use the style parameter to process the image, replace `image/resize,w_100` with `style/stylename`.

## Precautions

-   To perform the saveas operation, you must have permissions to write the requested bucket and object.
-   You must call ImgSaveAs to implement the saveas operation. To call this operation, you must have the `oss:PostProcessTask` permission. For more information, see [Implement access control based on RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Implement access control based on RAM policies.md).
-   The names of the requested bucket and object must comply with the naming conventions of OSS.
-   The requested bucket and object must be located within the same region.
-   The saveas operation applies only to the PostObject request. You cannot perform this operation in the GET request.

