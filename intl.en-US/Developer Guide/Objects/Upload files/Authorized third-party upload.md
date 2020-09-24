# Authorized third-party upload

This topic describes how to authorize a third-party user to upload objects directly to OSS without forwarding objects by using the server.

## Scenarios

In a standard client/server system architecture, the server is responsible for receiving and processing requests from the client. If OSS is used as a backend storage service, the client sends objects you want to upload to the application server. The server then forwards the objects to OSS. In this process, the data needs to be transmitted twice: from the client to the server and from the server to OSS. In the case of bursts of access requests, the server must provide sufficient bandwidth resources for multiple clients to upload objects simultaneously. This presents a challenge to the architecture scalability.

To resolve this issue, OSS provides authorized third-party upload. By using this feature, each client can upload objects directly to OSS without transmitting them to the server. This reduces the cost for application servers and maximizes the OSS capability to process large amounts of data. In this case, you can focus on your business, and do not need to worry about the bandwidth and concurrency limits.

OSS supports two methods to grant upload permissions: signed URL and temporary access credential.

## Signed URL

In this method, you can use a request URL that contains the OSS AccessKeyId and Signature fields to directly upload objects. Each signed URL has expiration time to ensure security.

-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Authorized access.md)|SDK demos for various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Authorized access.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Authorized access.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Authorized access.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Authorized access.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Authorized access.md)|

-   For more information, see [Generate a signed URL](/intl.en-US/API Reference/Access control/Generate a signed URL.md).

## Temporary access credential

Alibaba Cloud uses Security Token Service \(STS\) to grant temporary access credentials to authorize users. STS is a web service that provides temporary access tokens for cloud computing users. You can use STS to grant a third-party application or your RAM user an access credential that specifies the custom validity period and permissions.

-   Implementation modes

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Authorized access.md)|SDK demos for various programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Authorized access.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Authorized access.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Authorized access.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Authorized access.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Authorized access.md)|
    |[Android SDK](/intl.en-US/SDK Reference/Android/Authorized access.md)|
    |[iOS SDK](/intl.en-US/SDK Reference/iOS/Authorized access.md)|

-   For more information about implementation examples, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).

