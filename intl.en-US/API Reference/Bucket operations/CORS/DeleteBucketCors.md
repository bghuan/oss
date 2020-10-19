# DeleteBucketCors

You can call this operation to disable cross-origin resource sharing \(CORS\) for a specific bucket and delete all CORS rules configured for the bucket.

## Request structure

```
DELETE /? cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

DeleteBucketCors requests contain only common request headers. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

Responses for DeleteBucketCors requests contain only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

Sample request

```
DELETE /? cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:LnM4AZ1OeIduZF5vGFWicOME****
```

Sample response

```
HTTP/1.1 204 No Content 
x-oss-request-id: 5051845BC4689A033D00****
Date: Fri, 24 Feb 2012 05:45:34 GMT
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## SDK

You can use OSS SDKs for the following programming languages to call DeleteBucketCors:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/CORS.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/CORS.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/CORS.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/CORS.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/CORS.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/CORS.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/CORS.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/CORS.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/CORS.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to perform this operation. Only the owner of a bucket can delete the CORS rules configured for the bucket.|

