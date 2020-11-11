# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

For the complete IMG code, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/ImageSample.java).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    // Resize the image to a height and width of 100 pixels, name the image as example-resize.jpg, and save the image to your local computer.
    String style = "image/resize,m_fixed,w_100,h_100";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    ossClient.getObject(request, new File("example-resize.jpg"));
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Use different IMG parameters to process an image and save the image as the local image separately

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    // Resize the image to a height and width of 100 pixels, name the image as example-resize.jpg, and save the image to your local computer.
    String style = "image/resize,m_fixed,w_100,h_100";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    ossClient.getObject(request, new File("example-resize.jpg"));
    // Crop the image to a height and width of 100 pixels from the (100, 100) coordinate, name the image example-crop.jpg, and save the image to your local computer.
    style = "image/crop,w_100,h_100,x_100,y_100";
    request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    ossClient.getObject(request, new File("example-crop.jpg"));
    // Rotate the image 90°, name the image as example-rotate.jpg, and save the image to your local computer.
    style = "image/rotate,90";
    request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    ossClient.getObject(request, new File("example-rotate.jpg"));
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // After you resize the image to a height and width of 100 pixels, rotate the image 90°.
    String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image as example-rotate.jpg, and save the image to your local computer.
    ossClient.getObject(request, new File("example-new.jpg"));
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use image style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";

// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Specify the image style.
String style = "style/<yourCustomStyleName>";
GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
request.setProcess(style);
// Name the processed image as example-new.jpg, and save the image to your local computer.
ossClient.getObject(request, new File("example-new.jpg"));

// Shut down the OSSClient instance.
ossClient.shutdown();
```

## IMG persistence

By default, IMG does not save processed images. You can call the ImgSaveAs operation to save the images to the bucket where the source images are saved. The following code provides an example on how to save an processed image:

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
    // Resize the image to a height and width of 100 pixels, name the image as example-resize.png, and save the image to the bucket.
    StringBuilder sbStyle = new StringBuilder();
    Formatter styleFormatter = new Formatter(sbStyle);
    String styleType = "image/resize,m_fixed,w_100,h_100";
    String targetImage = "example-resize.png";
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

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you need to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String objectName = "<yourObjectName>";
// Create an OSSClient instance.
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
// After you resize the image to a height and width of 100 pixels, rotate the image 90°.
String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
// Set the validity period to 10 minutes.
Date expiration = new Date(new Date().getTime() + 1000 * 60 * 10 );
GeneratePresignedUrlRequest req = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.GET);
req.setExpiration(expiration);
req.setProcess(style);
URL signedUrl = ossClient.generatePresignedUrl(req);
System.out.println(signedUrl);
// Shut down the OSSClient instance.
ossClient.shutdown();
```

