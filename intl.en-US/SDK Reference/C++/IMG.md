# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
         /* Initialize the OSS account information. */
        std::string AccessKeyId = "<yourAccessKeyId>";
        std::string AccessKeySecret = "<yourAccessKeySecret>";
        std::string Endpoint = "<yourEndpoint>";
        std::string BucketName = "<yourBucketName>";
        std::string ObjectName = "<yourObjectName>";
    
         /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        /* After you resize the image to a height and width of 100 pixels, save the image to your local computer. */
        std::string Process = "image/resize,m_fixed,w_100,h_100";
        GetObjectRequest request(BucketName, ObjectName);
        request.setProcess(Process);
        auto outcome = client.GetObject(request);
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    #include <alibabacloud/oss/OssClient.h>
    using namespace AlibabaCloud::OSS;
    
    int main(void)
    {
         /* Initialize the OSS account information. */
        std::string AccessKeyId = "<yourAccessKeyId>";
        std::string AccessKeySecret = "<yourAccessKeySecret>";
        std::string Endpoint = "<yourEndpoint>";
        std::string BucketName = "<yourBucketName>";
        std::string ObjectName = "<yourObjectName>";
    
         /* Initialize network resources. */
        InitializeSdk();
    
        ClientConfiguration conf;
        OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    
        /* After you resize the image to a height and width of 100 pixels, rotate the image 90°, and save the image to your local computer. */
        std::string Process = "image/resize,m_fixed,w_100,h_100/rotate,90";
        GetObjectRequest request(BucketName, ObjectName);
        request.setProcess(Process);
        auto outcome = client.GetObject(request);
    
        /* Release network resources. */
        ShutdownSdk();
        return 0;
    }
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use the style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "<yourAccessKeyId>";
    std::string AccessKeySecret = "<yourAccessKeySecret>";
    std::string Endpoint = "<yourEndpoint>";
    std::string BucketName = "<yourBucketName>";
    std::string ObjectName = "<yourObjectName>";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf) ;

    /* Use the specified image style to process the image and save the image to your local computer. */
    std::string Process = "style/<yourCustomStyleName>";
    GetObjectRequest request(BucketName, ObjectName);
    request.setProcess(Process);
    auto outcome = client.GetObject(request);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## IMG persistence

You can call the ImgSaveAs operation to save the images to the bucket where the source images are stored. The following code provides an example on how to save an processed image:

```
#include <alibabacloud/oss/OssClient.h>
#include <sstream>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "<yourAccessKeyId>";
    std::string AccessKeySecret = "<yourAccessKeySecret>";
    std::string Endpoint = "<yourEndpoint>";
    std::string BucketName = "<yourBucketName>";
     /* The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg. */
    std::string SourceObjectName = "<yourSourceObjectName>";
     /* Specify the name of the processed image. */
    std::string TargetObjectName = "<yourTargetObjectName>";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /* Resize the image to a height and width of 100 pixels and save the image to the current bucket. */
    std::string Process = "image/resize,m_fixed,w_100,h_100";
    std::stringstream ss;
    ss  << Process 
    <<"|sys/saveas"
    << ",o_" << Base64EncodeUrlSafe(TargetObjectName)
    << ",b_" << Base64EncodeUrlSafe(BucketName);
    ProcessObjectRequest request(BucketName, SourceObjectName, ss.str());
    auto outcome = client.ProcessObject(request);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the OSS account information. */
    std::string AccessKeyId = "<yourAccessKeyId>";
    std::string AccessKeySecret = "<yourAccessKeySecret>";
    std::string Endpoint = "<yourEndpoint>";
    std::string BucketName = "<yourBucketName>";
    std::string ObjectName = "<yourObjectName>";

      /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);


    /* Generate a signed object URL that includes IMG parameters */
    std::string Process = "image/resize,m_fixed,w_100,h_100";
    GeneratePresignedUrlRequest request(BucketName, ObjectName, Http::Get);
    request.setProcess(Process);
    auto outcome = client.GeneratePresignedUrl(request);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

