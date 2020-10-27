# Error responses

If an error occurs when you access OSS, OSS returns an HTTP status code and an error message in the application/xml format so that you can identify and rectify the error.

## Response message body

Example of the message body of an error response:

```
<? xml version="1.0" ? >
<Error xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Code>
        AccessDenied
    </Code>
    <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
    </Message>
    <RequestId>
        1D842BC54255****
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

The message body of an error response includes the following elements:

-   Code: the error code that OSS returns to the user.
-   Message: the detailed error message provided by OSS.
-   RequestId: the UUID that uniquely identifies a request. If you need help from OSS development engineers to handle an exception, provide them with the RequestId value.
-   HostId: the ID of the host in the accessed OSS cluster, which is the same as the host ID specified in the request.

## Sample error request and response

If parameters that are not supported by OSS are added to requests to call operations supported by OSS \(for example, the If-Modified-Since parameter is added to the PUT operation request\), OSS returns `400 Bad Request`.

Sample error request

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2019 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2019 01:43:51 GMT
Content-Length: 363
```

Sample response

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2019 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955****
x-oss-server-time: 0
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented. </Message>
  <RequestId>57ABD896CCB80C366955****</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

For more information about OSS error codes, see [OSS error center](https://error-center.alibabacloud.com/status/product/Oss).

## Common errors and troubleshooting methods

-   [Errors returned with HTTP status code 203]()
-   [Errors returned with HTTP status code 400]()
-   [Errors returned with HTTP status code 403]()
-   [Errors returned with HTTP status code 404]()
-   [Errors returned with HTTP status code 405]()
-   [Errors returned with HTTP status code 409]()
-   [Errors returned with HTTP status code 411]()
-   [Errors returned with HTTP status code 412]()
-   [Errors returned with HTTP status code 503]()

## References

For more information about troubleshooting, you can view the FAQ topics for OSS features and tools. You can view the following FAQ topics for OSS SDKs for different programming languages and OSS tools to troubleshoot corresponding errors:

-   [Java](/intl.en-US/SDK Reference/Java/FAQ.md)
-   [Python](/intl.en-US/SDK Reference/Python/Common errors.md)
-   [Go](/intl.en-US/SDK Reference/Go/Error handling.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Exception handling.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/FAQ.md)
-   [ossbrowser](/intl.en-US/Tools/ossbrowser/FAQ.md)
-   [ossutil](/intl.en-US/Tools/ossutil/FAQ.md)
-   [ossfs](/intl.en-US/Tools/ossfs/FAQ.md)
-   [ossftp](/intl.en-US/Tools/ossftp/FAQ.md)

