# PutBucketAcl

You can call this operation to configure or modify the ACL of a bucket.

## Usage notes

When you call the PutBucketAcl operation, take note of the following items:

-   To call this operation, you must have write permissions on the bucket.
-   PutBucketAcl uses the overwriting semantics. A new ACL overwrites the existing one.
-   If the specified bucket for which you want to set ACL does not exist when you call this operation, a new bucket is created.

## Request structure

```
PUT /? acl HTTP/1.1
x-oss-acl: Permission
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

|Header|Type|Required|Description|
|:-----|:---|:-------|:----------|
|x-oss-acl|String|Yes|The ACL that you want to set for the bucket.This header is included in PutBucketAcl requests to set the ACL of the bucket. If this header is not included, the ACL settings do not take effect.

Valid values: public-read-write, public-read, and private

-   public-read-write: Any users, including anonymous users can read and write objects in the bucket. Exercise caution when you set the ACL of a bucket to public-read-write.
-   public-read: Only the owner or authorized users of this bucket can write objects in the bucket. Other users, including anonymous users can only read objects in the bucket. Exercise caution when you set the ACL of a bucket to public-read.
-   private: Only the owner or authorized users of this bucket can read and write objects in the bucket. Other users, including anonymous users cannot access the objects in the bucket without authorization. |

For the common request headers included in PutBucketAcl requests, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a PutBucketAcl request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

Sample request

```
PUT /? acl HTTP/1.1
x-oss-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT****
```

Sample response

-   Sample success response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Sample response to a request that includes invalid ACL settings

    ```
    HTTP/1.1 400 Bad Request
    x-oss-request-id: 56594298207FB3044385****
    Date: Fri, 24 Feb 2012 03:55:00 GMT
    Content-Length: 309
    Content-Type: text/xml; charset=UTF-8
    Connection: keep-alive
    Server: AliyunOSS
    
    <? xml version="1.0" encoding="UTF-8"? >
    <Error>
      <Code>InvalidArgument</Code>
      <Message>no such bucket access control exists</Message>
      <RequestId>5***9</RequestId>
      <HostId>***-test.example.com</HostId>
      <ArgumentName>x-oss-acl</ArgumentName>
      <ArgumentValue>error-acl</ArgumentValue>
    </Error>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutBucketAcl operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Manage bucket ACLs.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Manage bucket ACLs.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Manage bucket ACLs.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Manage bucket ACLs.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Manage bucket ACLs.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Manage bucket ACLs.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Manage bucket ACLs.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Manage bucket ACLs.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|AccessDenied|403|-   The error message returned because the information for user authentication is not included in the PutBucketAcl request.
-   The error message returned because you are not authorized to initiate a PutBucketAcl request. |

