# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
        HandleError(err)
        }
    
        // Specify the bucket where the source image is located.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
        HandleError(err)
        }
    
        // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
        sourceImageName := "<yourObjectName>"
        // Specify the name of the processed image.
        targetImageName := "<LocalFileName>"
        // Resize the image to a height and width of 100 pixels and save the image to your local computer.
        style := "image/resize,m_fixed,w_100,h_100"
        err = bucket.GetObjectToFile(sourceImageName, targetImageName, oss.Process(style))
        if err ! = nil {
        HandleError(err)
        }
    }
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    package main
    
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
        HandleError(err)
        }
    
        // Specify the bucket where the source image is located.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
        HandleError(err)
         }
    
        // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
        sourceImageName := "<yourObjectName>"
        // Specify the name of the processed image.
        targetImageName := "<LocalFileName>"
        // After you resize the image to a height and width of 100 pixels, rotate the image 90°, and save the image to your local computer.
        style := "image/resize,m_fixed,w_100,h_100/rotate,90"
        err = bucket.GetObjectToFile(sourceImageName, targetImageName, oss.Process(style))
        if err ! = nil {
         HandleError(err)
        }
    }
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use the style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func HandleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
    HandleError(err)
    }
    // Specify the bucket where the source image is located.
    bucketName := "<yourBucketName>"
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
     HandleError(err)
    }
    // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
    sourceImageName := "<yourObjectName>"
    // Specify the name of the processed image.
    targetImageName := "<LocalFileName>"
    // Specify the image style.
    style := "style/<yourCustomStyleName>"
    // Save the processed image to your local computer.
    err = bucket.GetObjectToFile(sourceImageName, targetImageName, oss.Process(style))
    if err ! = nil {
    HandleError(err)
    }
}
```

## IMG persistence

You can call the ImgSaveAs operation to save the processed images to the specified bucket. The following code provides an example on how to save an processed image:

-   Save the processed image to the current bucket

    ```
    package main
    
    import (
        "fmt"
        "os"
        "encoding/base64"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
        HandleError(err)
        }    
    
        // Specify the bucket where the source image is located.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
         HandleError(err)
        }
        // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
        sourceImageName := "<yourObjectName>"
        // Specify the name of the processed image.
        targetImageName := "<targetObjectName>"
        // Resize the image to a height and width of 100 pixels and save the image to the current bucket.
        style := "image/resize,m_fixed,w_100,h_100"
        process := fmt.Sprintf("%s|sys/saveas,o_%v", style, base64.URLEncoding.EncodeToString([]byte(targetImageName)))
        result, err := bucket.ProcessObject(sourceImageName, process)
        if err ! = nil {
        HandleError(err)
        } else {
            fmt.Println(result)
        }
    }
    ```

-   Save the processed image to the specified bucket

    ```
    package main
    
    import (
        "fmt"
        "os"
        "encoding/base64"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func HandleError(err error) {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
        HandleError(err)
        }    
    
        // Specify the bucket where the source image is located.
        bucketName := "<yourSourceBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
         HandleError(err)
        }
        // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
        sourceImageName := "<yourObjectName>"
        // Specify the bucket where the processed image is stored. The bucket must be within the same region as the source bucket.
        targetBucketName := "<TargetBucketName>"
        // Specify the name of the processed image.
        targetImageName = "<TargetObjectName>"
        // Resize the image to a height and width of 100 pixels and save the image to the specified bucket.
        style = "image/resize,m_fixed,w_100,h_100"
        process = fmt.Sprintf("%s|sys/saveas,o_%v,b_%v", style, base64.URLEncoding.EncodeToString([]byte(targetImageName)), base64.URLEncoding.EncodeToString([]byte(targetBucketName)))
        result, err = bucket.ProcessObject(sourceImageName, process)
        if err ! = nil {
        HandleError(err)
        } else {
            fmt.Println(result)
        }
    }
    ```


## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
package main

import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func HandleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
    HandleError(err)
    }

    // Specify the bucket where the source image is located.
    bucketName := "<yourBucketName>"
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
    HandleError(err)
    }
    // The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
    ossImageName := "<yourObjectName>"
    // Generate a signed object URL that includes IMG parameters. Set the validity period to 600 seconds.
    signedURL, err := bucket.SignURL(ossImageName, oss.HTTPGet, 600, oss.Process("image/format,png"))
    if err ! = nil {
    HandleError(err)
    } else {
    fmt.Println(signedURL)
    }
}
```

