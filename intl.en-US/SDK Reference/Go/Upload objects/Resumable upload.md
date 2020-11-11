# Resumable upload

Before you upload an object to OSS by using resumable upload, you can specify a folder for the checkpoint file that stores resumable upload records. If a file fails to be uploaded because the network is abnormal or the program stops responding, the upload task is resumed from the position recorded in the checkpoint file to upload remaining bytes.

## Background information

During resumable upload, the upload progress is recorded in a checkpoint file. If the upload of a part fails, the next upload starts from the position recorded in the checkpoint file. After resumable upload is complete, the checkpoint file is deleted.

**Note:**

-   The SDK records the upload progress in the checkpoint file. Ensure that you have write permissions on the checkpoint file. The checkpoint file contains a checksum. This checksum cannot be edited. If the checkpoint file is damaged, you must upload all parts again.
-   If the local file to upload is modified during the upload, you must upload the entire object again.

You can use the Bucket.UploadFile method to perform resumable upload. The following table describes the parameters you can configure for this operation.

|Parameter|Description|
|:--------|:----------|
|objectKey|The name of the object.|
|filePath|The local file path from which to upload the object.|
|partSize|The size of each part. Valid values: 100 KB to 5 GB. Default value: 100 KB.|
|options|Valid values: -   Routines: specifies the number of concurrent upload threads. The default value is 1, which indicates that only one thread is used to upload the parts.
-   Checkpoint: specifies whether resumable upload is enabled and the path of the checkpoint file. By default, resumable upload is disabled.

For example, `oss.Checkpoint(true, "")` indicates that resumable upload is enabled and the checkpoint file is the `file.cp` file contained in the same directory as the source local file. `file` is the name of the local file to upload. You can also use `oss.Checkpoint(true, "your-cp-file.cp")` to specify the checkpoint file.


 **Note:** For more information about other metadata, see [Manage object metadata](/intl.en-US/SDK Reference/Go/Manage objects/Manage object metadata.md). |

## Sample code

The following code provides an example on how to perform resumable upload by using multipart upload.

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

    // Specify the name of the bucket to which to upload the object.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    // Set the part size to 100 KB, set the number of concurrent upload threads to 3, and enable resumable upload.
    // yourObjectName is equivalent to the objectKey and indicates the complete path of the object you want to upload to OSS. The path must include the file extension of the object. For example, you can set yourObjectName to abc/efg/123.jpg.
    // Set filePath to LocalFile. Set partSize to 100*1024.
    err = bucket.UploadFile("<yourObjectName>", "LocalFile", 100*1024, oss.Routines(3), oss.Checkpoint(true, ""))
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}            
```

