# Image processing

Image Processing \(IMG\) is a secure, cost-effective, and highly reliable image processing service provided by OSS. IMG is capable of processing large amounts of images. After uploading source images to OSS, you can use Representational State Transfer \(RESTful\) APIs to process the images anytime, anywhere, and on any Internet device. You can create your own IMG service in OSS.

For more information about IMG, see [OSS Image Processing Guide](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

## Basic features

IMG has the following features:

-   [Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md)
-   [Crop images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Incircle.md)
-   [Rotate images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Automatic rotation.md)
-   [Apply effects](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Brightness.md)
-   [Convert formats](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Format conversion.md)
-   [Add watermarks](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Add watermarks.md): allows you to add image, text, or image-text watermarks to another image.

## How to use IMG

IMG uses the standard HTTP GET request. You can configure IMG parameters in QueryString of a URL.

If the ACL of an image object is Private, only authorized users are allowed access.

-   Anonymous access

    ```
    OSSTask * task = [_client presignPublicURLWithBucketName:_publicBucketName
                                              withObjectKey:@"hasky.jpeg"
                                             withParameters:@{@"x-oss-process": @"image/resize,w_50"}];
    ```

-   Authorized access

    To use IMG in the SDK, you can call the `request.setxOssProcess()` method to set IMG parameters when downloading an image. Example:

    ```
    OSSGetObjectRequest * request = [OSSGetObjectRequest new];
    request.bucketName = @"<bucketName>";
    request.objectKey = @"example.jpg";
    // Process the image.
    request.xOssProcess = @"image/resize,m_lfit,w_100,h_100";
    OSSTask * getTask = [client getObject:request];
    [getTask continueWithBlock:^id(OSSTask *task) {
        if (! task.error) {
            NSLog(@"download image success!") ;
            OSSGetObjectResult * getResult = task.result;
            NSLog(@"download image data: %@", getResult.dowloadedData);
        } else {
            NSLog(@"download object failed, error: %@" ,task.error);
        }
        return nil;
    }];
    // [getTask waitUntilFinished];
    // [request cancel];
    ```

    **Note:** To perform other processing operations on the image, you need only to replace relevant parameters in `request.setxOssProcess()`.

-   Access with the SDK

    ```
    OSSGetObjectRequest * request = [OSSGetObjectRequest new];
    request.bucketName = @"bucket-name";    // Set the bucket name.
    request.objectKey = @"image-name";      // Set the image name.
    request.xOssProcess = @"image/resize,m_lfit,w_100,h_100";   // Process the image.
    OSSTask * task = [ossClient getObject:request];
    ```


## Implement persistent storage for a processed image

Use the following code to store a processed image in a bucket for persistent storage:

```
OSSImagePersistRequest *request = [OSSImagePersistRequest new];
request.fromBucket = _privateBucketName;
request.fromObject = OSS_IMAGE_KEY;
request.toBucket = _privateBucketName;
request.toObject = @"image_persist_key";   
Request. action = @ "image/resize, w_100"; // Process the image.
//request.action = @"resize,w_100";

[[[ossClient imageActionPersist:request] continueWithBlock:^id _Nullable(OSSTask * _Nonnull task) {

    return nil;
}] waitUntilFinished];
```

