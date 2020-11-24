# Append upload

You can use AppendObject to append content to append objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.``
-   The CopyObject operation cannot be performed on append objects.

## Sample codes

The following code provides an example on how to perform append upload:

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

    // Obtain the bucket.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    var nextPos int64 = 0
    // If the object is appended for the first time, the append position is 0. The returned value is the position for the next append. The position to start the next append is the total length of appended bytes and the append object.
    // <yourObjectName> indicates the complete path of the object you want to append, and must include the extension of the object. Example: example/test.txt
    nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue1"), nextPos)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    
    // If the object is not appended for the first time, you can obtain the append position by using bucket.GetObjectDetailedMeta or querying the value of X-Oss-Next-Append-Position returned in the last append upload.
    //props, err := bucket.GetObjectDetailedMeta("<yourObjectName>")
    //if err ! = nil {
    //    fmt.Println("Error:", err)
    //    os.Exit(-1)
    //}
    //nextPos, err = strconv.ParseInt(props.Get("X-Oss-Next-Append-Position"), 10, 64)
    //if err ! = nil {
    //    fmt.Println("Error:", err)
    //    os.Exit(-1)
    //}    

    // Perform the second append upload.
    nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue2"), nextPos)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // You can perform append upload multiple times.
}
```

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

