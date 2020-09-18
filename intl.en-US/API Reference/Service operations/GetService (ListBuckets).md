# GetService \(ListBuckets\)

You can call this operation to obtain all buckets that you have. The forward slash \(/\) in the request syntax represents the root folder.

**Note:** The GetService \(ListBuckets\) operation is valid only for authenticated users.

## Request structure

```
GET / HTTP/1.1
Host: oss.example.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters

**Note:** When you use GetService \(ListBuckets\), you can set the parameters described in the following table to limit the list of buckets returned so that only specified results are returned.

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|prefix|String|No|Specifies the prefix that returned bucket names must contain. If this parameter is not specified, prefix information is not used to filter the returned buckets. Default value: null |
|marker|String|No|Specifies the name of the bucket after which the list begins. If this parameter is not specified, all results are returned. Default value: null |
|max-keys|Integer|No|Specifies the maximum number of buckets that can be returned each time. If this parameter is not specified, the default value is 100. The maximum value is 1000. Default value: 100 |

## Response elements

**Note:** When all buckets are returned, the returned XML does not contain Prefix, Marker, MaxKeys, IsTruncated, or NextMarker. If a part of results are not returned, the preceding nodes are added.

|Element|Type|Description|
|-------|----|-----------|
|ListAllMyBucketsResult|Container|The container used to store results of the GetService request. Child nodes: Owner and Buckets

Parent nodes: none |
|Prefix|String|The prefix that the returned object names must contain. Parent nodes: ListAllMyBucketsResult |
|Marker|String|The name of the bucket after which the list begins. Parent nodes: ListAllMyBucketsResult |
|MaxKeys|String|The maximum number of buckets returned each time. Parent nodes: ListAllMyBucketsResult |
|IsTruncated|Enumerated string|Specifies whether all results are returned. Valid values: true and false

-   true indicates that not all of the results are returned this time.
-   false indicates that all of the results are returned this time.

Parent nodes: ListAllMyBucketsResult |
|NextMarker|String|The marker for the next GetService \(ListBuckets\) request, which can be used to return the results that are not returned this time. Parent nodes: ListAllMyBucketsResult |
|Owner|Container|The container used to store the information of the bucket owner. Parent nodes: ListAllMyBucketsResult |
|ID|String|The user ID of the bucket owner. Parent nodes: ListAllMyBucketsResult and Owner |
|DisplayName|String|The name of the bucket owner, which is the same as the user ID. Parent nodes: ListAllMyBucketsResult and Owner |
|Buckets|Container|The container that stores the information about multiple buckets. Child nodes: Bucket

Parent nodes: ListAllMyBucketsResult |
|Bucket|Container|The container used to store the bucket information. Child nodes: Name, CreationDate, and Location

Parent nodes: ListAllMyBucketsResult and Buckets |
|Name|String|The name of the bucket. Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |
|CreateDate|Time \(Format: yyyy-mm-ddThh:mm:ss.timezone. Example: 2011-12-01T12:27:13.000Z\)|The time when the bucket is created. Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |
|Location|String|The data center in which the bucket is located. Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |
|ExtranetEndpoint|String|The public endpoint used to access the bucket over the Internet. Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |
|IntranetEndpoint|String|The internal endpoint used to access the bucket from ECS instances in the same region. Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |
|StorageClass|String|The storage class of the bucket.Valid values: Standard, IA, Archive, and ColdArchive

Parent nodes: ListAllMyBucketsResult, Buckets, and Bucket |

## Examples

-   Sample Request 1

    ```
    GET / HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Host: oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS nxj7dtlhcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2******
    ```

    Sample Response 1

    ```
    HTTP/1.1 200 OK
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Type: application/xml
    Content-Length: 556
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C2300****
    <? xml version="1.0" encoding="UTF-8"? >
    <ListAllMyBucketsResult>
      <Owner>
        <ID>512**</ID>
        <DisplayName>51264</DisplayName>
      </Owner>
      <Buckets>
        <Bucket>
          <CreationDate>2015-12-17T18:12:43.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-shanghai.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-shanghai-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-shanghai</Location>
          <Name>app-base-oss</Name>
          <Region>cn-shanghai</Region>
          <StorageClass>Standard</StorageClass>
        </Bucket>
        <Bucket>
          <CreationDate>2014-12-25T11:21:04.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-hangzhou</Location>
          <Name>atestleo23</Name>
          <Region>cn-hangzhou</Region>
          <StorageClass>IA</StorageClass>
        </Bucket>
      </Buckets>
    </ListAllMyBucketsResult>
    ```

-   Sample Request 2

    ```
    GET /? prefix=xz02tphky6fjfiuc&max-keys=1 HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Host: oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS nxj7dtwhcyl5hpvnhi:COS3OQkfQPnKmYZTEHYv2****
    ```

    Sample Response 2

    ```
    HTTP/1.1 200 OK
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Type: application/xml
    Content-Length: 545
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C2300****
    <? xml version="1.0" encoding="UTF-8"? >
    <ListAllMyBucketsResult>
      <Prefix>xz02tphky6fjfiuc</Prefix>
      <Marker></Marker>
      <MaxKeys>1</MaxKeys>
      <IsTruncated>true</IsTruncated>
      <NextMarker>xz02tphky6fjfiuc0</NextMarker>
      <Owner>
        <ID>ut_test_put_bucket</ID>
        <DisplayName>ut_test_put_bucket</DisplayName>
      </Owner>
      <Buckets>
        <Bucket>
          <CreationDate>2014-05-15T11:18:32.000Z</CreationDate>
          <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
          <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
          <Location>oss-cn-hangzhou</Location>
          <Name>xz02tphky6fjfiuc0</Name>
          <Region>cn-hangzhou</Region>
          <StorageClass>Standard</StorageClass>
        </Bucket>
      </Buckets>
    </ListAllMyBucketsResult>
    ```


## SDK

SDKs of the GetService \(ListBuckets\) operation for various programming languages:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Manage buckets.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Create buckets.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|403|The error message returned because the request is from anonymous access and includes no user authentication information.|

