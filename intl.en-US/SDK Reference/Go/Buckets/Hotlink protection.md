# Hotlink protection

To prevent your data on OSS from being leeched, OSS supports hotlink protection through the referer field settings in the HTTP header, including the following parameters:

-   Referer whitelist: Used to allow access only for specified domains to OSS data.
-   Empty referer: Determines whether the referer can be empty. If it is not allowed, only requests with the referer filed in their HTTP or HTTPS headers can access OSS data.

For more information about hotlink protection, see [Configure hotlink protection](/intl.en-US/Developer Guide/Data security/Access and control/Configure hotlink protection.md). For the complete code of configuring hotlink protection, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/bucket_referer.go).

## Configure the referer whitelist

Run the following code to configure the referer whitelist:

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
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"

    // Add the referer field. Empty referer is not allowed. The referer field allows question marks (?) and asterisks (*) for wildcard use.
    referers := []string{"http://www.aliyun.com",
                         "http://www.???.aliyuncs.com",
                         "http://www. *.com"}
    err = client.SetBucketReferer(bucketName, referers, false)
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

## Obtain a referer whiltelist

Run the following code to obtain a referer whiltelist:

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
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"

    // Obtain the referer whitelist.
    refRes, err := client.GetBucketReferer(bucketName)
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("Referers: ", refRes.RefererList)
    fmt.Println("AllowEmptyReferer: ", refRes.AllowEmptyReferer)
}
            
```

## Clear a referer whitelist

Run the following code to clear a referer whitelist:

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
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }

    bucketName := "<yourBucketName>"

    // Clear the referer whitelist. // You cannot clear a referer whitelist directly. To clear a referer whitelist, you need to create the rule that allows an empty referer field and replace the original rule with the new rule.
    err = client.SetBucketReferer(bucketName, []string{}, true)
    if err!=nil{
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
            
```

