# GetBucketV2 \(ListObjectsV2\)

You can call this operation to query information about all objects in a bucket.

**Note:** The user metadata is not returned for the GetBucketV2 \(ListObjectsV2\) request.

## Request structure

```
GET /? list-type=2 HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|List-type|Number|Yes|The version of the ListObjects operation. Set the value to 2.|
|Delimiter|String|No|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Default value: none |
|Start-after|String|No|Specifies the Start-after value from which to start the list. The names of objects are returned in alphabetical order. The Start-after parameter is used to list the returned objects by page. This parameter value must be smaller than 1,024 bytes in length.

Even if the list excludes the object name specified by Star-after during a conditional query, the names of objects from which to start the list are returned in alphabetical order.

Default value: none |
|Continuation-token|String|No|The token from which the List operation must start. You can query the token from NextContinuationToken in the ListObjectsV2 result.|
|Max-keys|String|No|The maximum number of objects to return. Valid values: 1 to 1000

Default value: 100

**Note:** If the number of objects exceeds the Max-keys value, the response includes `<NextContinuationToken>` as the continuation token for next listing. |
|Prefix|String|No|The prefix that the returned object names must contain. If Prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

If you specify Prefix and set Delimiter to a forward slash \(/\), only the objects in the folder are listed. The subfolders are grouped together as a single result in CommonPrefixes. Objects and folders in the subfolders are not listed.

For example, a bucket contains the following objects: fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If Prefix is set to fun/, the three objects are returned. If Prefix is set to fun/ and Delimiter is set to a forward slash \(/\), fun/test.jpg and fun/movie/are returned.

**Note:**

-   The parameter value must be smaller than 1,024 bytes in length.
-   If you specify a prefix to query objects, the names of the returned objects still contain the prefix.

Default value: none |
|Encoding-type|String|No|The encoding type of the object name in the response. Default value: none

Valid value: url

**Note:** The Delimiter, Start-after, Prefix, NextContinuationToken, and Key values are UTF-8 encoded. If Delimiter, Start-after, Prefix, NextContinuationToken, and Key contain control characters that are not supported by the XML 1.0 standard, you can specify Encoding-type to encode Delimiter, Start-after, Prefix, NextContinuationToken, and Key in the response. |
|Fetch-owner|Boolean|No|Specifies whether to include the owner information in the response. Valid values: true and false

-   true: The response includes the owner information.
-   false: The response excludes the owner information.

Default value: false |

## Response elements

|Element|Type|Description|
|-------|----|-----------|
|Contents|Container|The container that stores the returned object metadata. Parent nodes: ListBucketResult |
|CommonPrefixes|String|If the Delimiter parameter is specified in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent nodes: ListBucketResult |
|Delimiter|String|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent nodes: ListBucketResult |
|EncodingType|String|The encoding type of the object name in the response. If you specify Encoding-type in the request, Delimiter, StartAfter, Prefix, NextContinuationToken, and Key are encoded in the response. Parent nodes: ListBucketResult |
|DisplayName|String|The name of the object owner. Parent nodes: ListBucketResult, Contents, and Owner |
|ETag|String|The entity tag \(ETag\). An ETag is created when the object is created to identify the content of an object. Parent nodes: ListBucketResult and Contents

-   If an object is created by using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created by using other methods, the ETag value is the UUID of the object content.
-   The ETag value of the object can be used to check whether the object content is modified. However, we recommend that you do not use the ETag of an object as the MD5 hash of the object to verify data integrity. |
|ID|String|The user ID of the bucket owner. Parent nodes: ListBucketResult, Contents, and Owner |
|IsTruncated|Enumerated string|Indicates whether the returned results are truncated. Valid values: true and false

-   true indicates that not all of the results are returned this time.
-   false indicates that all of the results are returned this time.

Parent nodes: ListBucketResult |
|Key|String|The key of the object. Parent nodes: ListBucketResult and Contents |
|LastModified|Date|The last modified time of the object. Parent nodes: ListBucketResult and Contents |
|ListBucketResult|Container|The container that stores the result of the GetBucket request. Child nodes: Name, Prefix, StartAfter, MaxKeys, Delimiter, IsTruncated, NextContinuationToken, and Contents

Parent nodes: none |
|StartAfter|String|If the StartAfter parameter is specified in the request, the response contains the StartAfter element.|
|MaxKeys|String|The maximum number of buckets returned each time. Parent nodes: ListBucketResult |
|Name|String|The name of the bucket. Parent nodes: ListBucketResult |
|Owner|Container|The container that stores the information about the bucket owner. Child nodes: DisplayName and ID

Parent nodes: ListBucketResult |
|Prefix|String|The prefix that the returned object names must contain. Parent nodes: ListBucketResult |
|Size|String|The size of the returned object content. Unit: bytes. Parent nodes: ListBucketResult and Contents |
|StorageClass|String|The storage class of the object. Parent nodes: ListBucketResult and Contents |
|ContinuationToken|String|If the ContinuationToken parameter is specified in the request, the response contains the ContinuationToken element.|
|KeyCount|Number|The number of keys returned for this request. If Delimiter is specified, KeyCount is the sum of the elements in Key and CommonPrefixes.|
|NextContinuationToken|String|The name of the object from which the subsequent list begins. The value of NextContinuationToken is used as that of ContinuationToken to query subsequent results.|

## Examples

-   Sample requests for simple GetBucket

    ```
    GET /? list-type=2 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <MaxKeys>100</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a</Key>
            <LastModified>2020-05-18T05:45:43.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>b</Key>
            <LastModified>2020-05-18T05:45:50.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>b/c</Key>
            <LastModified>2020-05-18T05:45:54.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>bc</Key>
            <LastModified>2020-05-18T05:45:59.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>c</Key>
            <LastModified>2020-05-18T05:45:57.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <KeyCount>6</KeyCount>
    </ListBucketResult>
    ```

-   Sample requests that have the Prefix parameter specified

    ```
    GET /? list-type=2&prefix=a HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix>a</Prefix>
        <MaxKeys>100</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a</Key>
            <LastModified>2020-05-18T05:45:43.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <KeyCount>2</KeyCount>
    </ListBucketResult>
    ```

-   Sample requests that have the Prefix and Delimiter parameters specified

    ```
    GET /? list-type=2&prefix=a/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix>a/</Prefix>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
        <EncodingType>url</EncodingType>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>a/b</Key>
            <LastModified>2020-05-18T05:45:47.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
        </Contents>
        <CommonPrefixes>
            <Prefix>a/b/</Prefix>
        </CommonPrefixes>
        <KeyCount>2</KeyCount>
    </ListBucketResult>
    ```

-   Sample requests that have the StartAfter, MaxKeys, and FetchOwner parameters specified

    ```
    GET /? list-type=2&start-after=b&max=keys=3&fetch-owner=true HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <StartAfter>b</StartAfter>
        <MaxKeys>3</MaxKeys>
        <EncodingType>url</EncodingType>
        <IsTruncated>true</IsTruncated>
        <NextContinuationToken>CgJiYw--</NextContinuationToken>
        <Contents>
            <Key>b/c</Key>
            <LastModified>2020-05-18T05:45:54.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>ba</Key>
            <LastModified>2020-05-18T11:17:58.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>bc</Key>
            <LastModified>2020-05-18T05:45:59.000Z</LastModified>
            <ETag>"35A27C2B9EAEEB6F48FD7FB5861D****"</ETag>
            <Size>25</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1686240967192623</ID>
                <DisplayName>1686240967192623</DisplayName>
            </Owner>
        </Contents>
        <KeyCount>3</KeyCount>
    </ListBucketResult>
    ```


## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to view information about the bucket.|
|InvalidArgument|400|-   The error message returned because the value of Max-keys is smaller than 0 or greater than 1000.
-   The error message returned because the length of the value of Prefix, Start-after, or Delimiter is invalid. |

