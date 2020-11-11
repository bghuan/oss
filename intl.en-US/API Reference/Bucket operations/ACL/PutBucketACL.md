# PutBucketACL

You can call this operation to configure or modify the ACL of a bucket.

## Usage notes

When you call the PutBucketACL operation, take note of the following items:

-   The requester to call this operation, must have write permissions on the bucket.
-   PutBucketACL uses the overwriting semantics. A new ACL overwrites the existing one.
-   If the specified bucket for which you want to set the ACL does not exist, a new bucket is created when you call this operation.

## Request syntax

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
|x-oss-acl|String|Yes|The ACL for a bucket.This parameter is included in the PutBucketACL request to set the ACL for the bucket. If this header is not included, the ACL settings do not take effect.

Valid values: public-read-write, public-read, and private

-   public-read-write: Any users, including anonymous users can read and write objects in the bucket. Exercise caution when you set the ACL to public read/write.
-   public-read: Only the owner or authorized users of this bucket can write objects in the bucket. Other users, including anonymous users can only read objects in the bucket. Exercise caution when you set the ACL to public read.
-   private: Only the owner or authorized users of this bucket can read and write objects in the bucket. Other users, including anonymous users cannot access the objects in the bucket without authorization. |

## Examples

Sample requests

```
PUT /? acl HTTP/1.1
x-oss-acl: public-read
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 03:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT****
```

Sample responses

-   Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Sample responses to requests that include invalid ACL settings:

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

SDKs of the PutBucketACL operation for various programming languages:

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
|AccessDenied|403|-   The error message returned because the information for user authentication is not included when you initiate a PutBucketACL request.
-   The error message returned because you are not authorized to initiate a PutBucketACL request. |

