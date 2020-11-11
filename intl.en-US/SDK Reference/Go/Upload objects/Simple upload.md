# Simple upload

This topic describes how to use simple upload.

For the complete code used to perform simple upload, visit [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/put_object.go).

## Upload a string

The following code provides an example on how to upload a string:

```
package main

import (
    "fmt"
    "os"
    "strings"

    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Specify the bucket name.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Set the storage class to Standard. The default storage class is Standard.
    storageType := oss.ObjectStorageClass(oss.StorageStandard)

    // Set the storage class to Archive.
    // storageType := oss.ObjectStorageClass(oss.StorageArchive)

    // Set the ACL to public read. By default, the object inherits the bucket ACL.
    objectAcl := oss.ObjectACL(oss.ACLPublicRead)

    // Upload the string.
    err = bucket.PutObject("<yourObjectName>", strings.NewReader("yourObjectValue"), storageType, objectAcl)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```

## Upload a byte array

The following code provides an example on how to upload a byte array:

```
package main

import (
    "fmt"
    "os"
    "bytes"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Specify the bucket name.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Upload the byte array.
    err = bucket.PutObject("<yourObjectName>", bytes.NewReader([]byte("yourObjectValueByteArrary")))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

## Upload a file stream

The following code provides an example on how to upload a file stream:

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

    // Specify the bucket name.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Read the local file.
    fd, err := os.Open("<yourLocalFile>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    defer fd.Close()

    // Upload the file stream.
    err = bucket.PutObject("<yourObjectName>", fd)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

## Upload a local file

The following code provides an example on how to upload a local file to OSS:

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

    // Specify the bucket name.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Upload the local file.
    err = bucket.PutObjectFromFile("<yourObjectName>", "<yourLocalFile>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

