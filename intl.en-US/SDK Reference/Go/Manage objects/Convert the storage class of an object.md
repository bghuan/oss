# Convert the storage class of an object

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), Archive and Cold Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md) and [Convert storage classes](/intl.en-US/Developer Guide/Storage classes/Convert storage classes.md) in the OSS Developer Guide.

The following code provides an example of how to convert the storage class of an object.

-   The following code provides an example of how to convert the storage class of an object from Standard or IA to Archive:

    ```
    package main
    
    import (
        "fmt"
        "os"
    
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    
    func main() {
        // Create an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        bucketName := "<yourBucketName>"
        objectName := "<yourObjectName>"
    
        // Obtain information about the bucket based on the bucket name.
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    
        // Modify the storage class of the object. Set the storage class to Archive.
        _, err = bucket.CopyObject(objectName, objectName, oss.ObjectStorageClass(oss.StorageArchive))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
    }
    				
    ```

-   The following code provides an example of how to convert the storage class from Archive to Standard:

```
package main

import (
    "fmt"
    "os"
    "time"

    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"

    // Obtain information about the bucket based on the bucket name.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Check whether the storage class of the source object is Archive. If the storage class of the source object is Archive, you must restore the object before you can modify the storage class.
    meta, err := bucket.GetObjectDetailedMeta(objectName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    fmt.Println("X-Oss-Storage-Class : ", meta.Get(oss.HTTPHeaderOssStorageClass))
    if meta.Get(oss.HTTPHeaderOssStorageClass) == string(oss.StorageArchive) {
        // Restore the object.
        err = bucket.RestoreObject(objectName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }

        // Wait for about one minute until the object is restored.
        meta, err = bucket.GetObjectDetailedMeta(objectName)
        for meta.Get("X-Oss-Restore") == "ongoing-request=\"true\"" {
            fmt.Println("X-Oss-Restore:" + meta.Get("X-Oss-Restore"))
            time.Sleep(1 * time.Second)
            meta, err = bucket.GetObjectDetailedMeta(objectName)
        }
    }

    //Modify the storage class of the restored object. Set the storage class to Standard.
    _, err = bucket.CopyObject(objectName, objectName, oss.ObjectStorageClass(oss.StorageStandard))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```


For more information about the storage classes and storage fees, see the [t4320.md\#section\_jmi\_2z4\_hka](/intl.en-US/Pricing/Billing items and methods/Overview.md) section in Billing items.

