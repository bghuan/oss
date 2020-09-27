# Download objects

If you call GetObject to download an object in a bucket with versioning enabled or suspended, only the current version of the object is returned by default.

When you perform the GetObject operation on an object in a bucket:

-   If the current version of the object is a delete marker, the 4040 Not Found error is returned.
-   If you specify the versionId of the target object in the request, the specified version of the object is returned. For example, if the versionId is specified as null in the request, the version of which the versionId is null is returned.
-   If the version specified in the request is a delete marker, the 405 Method Not Allowed error is returned.

You can run the following code to download an object:

```
package main

import (
  "fmt"
  "net/http"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains the bucket.
  bucket, err := client.Bucket("<yourBucketName>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  var retHeader http.Header
  // Downloads the object with the version ID: yourObjectVersionId to the cache.
  _, err = bucket.GetObject("youObjectName", oss.VersionId("yourObjectVersionId"), oss.GetResponseHeader(&retHeader))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Prints x-oss-version-id.
  fmt.Println("x-oss-version-id:",  oss.GetVersionId(retHeader))
}
```

