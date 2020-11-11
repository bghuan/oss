# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    using System;
    using System.IO;
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    using Aliyun.OSS.Util;
    namespace ImageProcess
    {
        class Program
        {
            static void Main(string[] args)
            {
                Program.ImageProcess();
                Console.ReadKey();
            }
            public static void ImageProcess()
            {
                var endpoint = "<yourEndpoint>";
                var accessKeyId = "<yourAccessKeyId>";
                var accessKeySecret = "<yourAccessKeySecret>";
                var bucketName = "<yourBucketName>";
                var objectName = "<yourObjectName>";
                // The name of the local file when you upload local images.
                // var localImageFilename = "<yourLocalImageFilename>";
                // Specify the local folder where the processed image is saved.
                var downloadDir = "<yourDownloadDir>";
                // Create an OSSClient instance.
                var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                try
                {
                    // If the image does not exist in the required bucket, you must upload the image to the required bucket.   
                    // client.PutObject(bucketName, objectName, localImageFilename);
                    // Resize the image to a height and width of 100 pixels.
                    var process = "image/resize,m_fixed,w_100,h_100";
                    var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                    // Specify the name of the processed image.
                    WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                }
                catch (OssException ex)
                {
                    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Failed with error info: {0}", ex.Message);
                }
            }
            private static void WriteToFile(string filePath, Stream stream)
            {
                using (var requestStream = stream)
                {
                    using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                    {
                        IoUtils.WriteTo(stream, fs);
                    }
                }
            }
        }
    }
    ```

-   Use different IMG parameters to process an image and save each image as a local image

    ```
    using System;
    using System.IO;
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    using Aliyun.OSS.Util;
    namespace ImageProcess
    {
        class Program
        {
            static void Main(string[] args)
            {
                Program.ImageProcess();
                Console.ReadKey();
            }
            public static void ImageProcess()
            {
                var endpoint = "<yourEndpoint>";
                var accessKeyId = "<yourAccessKeyId>";
                var accessKeySecret = "<yourAccessKeySecret>";
                var bucketName = "<yourBucketName>";
                var objectName = "<yourObjectName>";
                // The name of the local file when you upload local images.
                // var localImageFilename = "<yourLocalImageFilename>";
                // Specify the local folder where the processed image is saved.
                var downloadDir = "<yourDownloadDir>";
                // Create an OSSClient instance.
                var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                try
                {
                    // If the image does not exist in the required bucket, you must upload the image to the required bucket.   
                    // client.PutObject(bucketName, objectName, localImageFilename);
                    // Resize the image to a height and width of 100 pixels.
                    var process = "image/resize,m_fixed,w_100,h_100";
                    var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                    // Specify the name of the processed image.
                    WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                    // Crop the image to a height and width of 100 pixels from the (100, 100) coordinate pair.
                    process = "image/crop,w_100,h_100,x_100,y_100";
                    ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                    WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                    // Rotate the image 90°.
                    process = "image/rotate,90";
                    ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                    WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                }
                catch (OssException ex)
                {
                    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Failed with error info: {0}", ex.Message);
                }
            }
            private static void WriteToFile(string filePath, Stream stream)
            {
                using (var requestStream = stream)
                {
                    using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                    {
                        IoUtils.WriteTo(stream, fs);
                    }
                }
            }
        }
    }
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    using System;
    using System.IO;
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    using Aliyun.OSS.Util;
    namespace ImageProcessCascade
    {
        class Program
        {
            static void Main(string[] args)
            {
                Program.ImageProcessCascade();
                Console.ReadKey();
            }
            public static void ImageProcessCascade()
            {
                var endpoint = "<yourEndpoint>";
                var accessKeyId = "<yourAccessKeyId>";
                var accessKeySecret = "<yourAccessKeySecret>";
                var bucketName = "<yourBucketName>";
                var objectName = "<yourObjectName>";
                // The name of the local file when you upload local images.
                // var localImageFilename = "<yourLocalImageFilename>";
                // Specify the local folder where the processed image is saved.
                var downloadDir = "<yourDownloadDir>";
                // Create an OSSClient instance.
                var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
                try
                {
                    // If the image does not exist in the required bucket, you must upload the image to the required bucket.   
                    // client.PutObject(bucketName, objectName, localImageFilename);
                    // After you resize the image to a height and width of 100 pixels, rotate the image 90°.
                    var process = "image/resize,m_fixed,w_100,h_100/rotate,90";
                    var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                    // Specify the name of the processed image.
                    WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                    Console.WriteLine("Get Object:{0} with process:{1} succeeded ", objectName, process);
                }
                catch (OssException ex)
                {
                    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Failed with error info: {0}", ex.Message);
                }
            }
            private static void WriteToFile(string filePath, Stream stream)
            {
                using (var requestStream = stream)
                {
                    using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                    {
                        IoUtils.WriteTo(stream, fs);
                    }
                }
            }
        }
    }
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use the style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
using System;
using System.IO;
using Aliyun.OSS;
using Aliyun.OSS.Common;
using Aliyun.OSS.Util;
namespace ImageProcessCustom
{
    class Program
    {
        static void Main(string[] args)
        {
            Program.ImageProcessCustomStyle();
            Console.ReadKey();
        }
        public static void ImageProcessCustomStyle()
        {
            var endpoint = "<yourEndpoint>";
            var accessKeyId = "<yourAccessKeyId>";
            var accessKeySecret = "<yourAccessKeySecret>";
            var bucketName = "<yourBucketName>";
            var objectName = "<yourObjectName>";
            // The name of the local file when you upload local images.
            // var localImageFilename = "<yourLocalImageFilename>";
            // Specify the local folder where the processed image is saved.
            var downloadDir = "<yourDownloadDir>";
            // Create an OSSClient instance.
            var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
            try
            {
                // If the image does not exist in the required bucket, you must upload the image to the required bucket.   
                // client.PutObject(bucketName, objectName, localImageFilename);
                // Use the specified image style to process the image.
                var process = "style/<yourCustomStyleName>";
                var ossObject = client.GetObject(new GetObjectRequest(bucketName, objectName, process));
                // Specify the name of the processed image.            
                WriteToFile(downloadDir + "/<LocalFilename>", ossObject.Content);
                Console.WriteLine("Get Object:{0} with process:{1} succeeded ", objectName, process);
            }
            catch (OssException ex)
            {
                Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
                    ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
            }
            catch (Exception ex)
            {
                Console.WriteLine("Failed with error info: {0}", ex.Message);
            }
        }
        private static void WriteToFile(string filePath, Stream stream)
        {
            using (var requestStream = stream)
            {
                using (var fs = File.Open(filePath, FileMode.OpenOrCreate))
                {
                    IoUtils.WriteTo(stream, fs);
                }
            }
        }
    }
}
```

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
using Aliyun.OSS;
using Aliyun.OSS.Common;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Resize the image to a height and width of 100 pixels.
    var process = "image/resize,m_fixed,w_100,h_100";
    var req = new GeneratePresignedUriRequest(bucketName, objectName, SignHttpMethod.Get)
    {
        Expiration = DateTime.Now.AddHours(1),
        Process = process
    };
    // Generate a signed URI.
    var uri = client.GeneratePresignedUri(req);
    Console.WriteLine("Generate Presigned Uri:{0} with process:{1} succeeded ", uri, process);
}
catch (OssException ex)
{
    Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
        ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

