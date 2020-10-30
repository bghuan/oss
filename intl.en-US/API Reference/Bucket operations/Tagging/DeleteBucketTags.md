# DeleteBucketTags

You can call this operation to delete tags configured for a bucket.

**Note:** If no tag is configured for the bucket or the tag to delete does not exist, OSS returns 204.

## Request structure

```
DELETE /? tagging HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

A DeleteBucketTags request contains only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a DeleteBucketTags request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample request sent to delete all tags configured for a bucket

    ```
    DELETE /? tagging HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    Sample response

    ```
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A****
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```

-   Sample request sent to delete tags of which the keys are k1 and k2

    ```
    DELETE /? tagging=k1,k2 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:6ZVHOehYzxoC1yxRydPQs/Cn****
    ```

    Sample response

    ```
    HTTP/1.1 204 No Content
    x-oss-request-id: 5C22E0EFD127F6810B1A92A8
    Date: Tue, 25 Dec 2018 17:35:24 GMT
    Connection: keep-alive
    Content-Length: 0
    Server: AliyunOSS
    ```


## SDK

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Bucket tagging.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Bucket tagging.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Bucket tagging.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Bucket tagging.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Bucket tagging.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Bucket tagging.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Bucket tagging.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|NoSuchBucket|404 No Content|The error message returned because the specified bucket does not exist.|
|AccessDenied|403 Forbidden|The error message returned because you do not have permissions to delete the tags configured for the bucket. Only the owner of a bucket can delete the tags configured for the bucket.|

