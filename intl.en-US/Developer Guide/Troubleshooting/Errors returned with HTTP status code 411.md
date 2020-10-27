# Errors returned with HTTP status code 411

This topic describes the causes of errors returned with HTTP status code 411 and the solutions to these errors.

## MissingContentLength

-   Error message: You must provide the Content-Length HTTP header.
-   Cause: The error message returned because the request does not use chunked encoding and does not contain the `Content-Length` header.
-   Solution: Ensure that the request uses [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) and contains the `Content-Length` header.

