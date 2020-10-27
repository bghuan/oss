# GetBucket \(ListObjects\)

You can call this operation to query information about all objects in a bucket.

**Note:**

-   The GetBucket \(ListObjects\) operation has been updated to GetBucketV2 \(ListObjectsV2\). We recommend that you use GetBucketV2 \(ListObjectsV2\) when you develop your applications. To provide backward compatibility, OSS continues to support the GetBucket \(ListObjects\) operation. For more information about GetBucketV2 \(ListObjectsV2\), see [GetBucketV2 \(ListObjectsV2\)]().
-   The user metadata is not returned for the GetBucket \(ListObjects\) request.

## Request syntax

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request parameters

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Delimiter|String|No|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Default value: none |
|Marker|String|No|The name of the object after which the list begins. If this parameter is specified, objects whose names are alphabetically greater than the Marker parameter value are returned. The Marker parameter is used to list the returned objects by page, and its value must be smaller than 1,024 bytes in length.

 Even if the specified marker does not exist in the list during a conditional query, the list starts from the object whose name is alphabetically greater than the Marker parameter value.

 Default value: none |
|Max-keys|String|No|The maximum number of objects to return. Valid values: 1 to 999

 Default value: 100

 **Note:** If the listing cannot be completed at one time because Max-keys is specified, a `<NextMarker>` is included in the response as the marker for next listing. |
|Prefix|String|No|The prefix that the returned object names must contain. If Prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

 If Prefix is specified and Delimiter is set to a forward slash \(/\), only the objects in the folder are listed. The subfolders are grouped together as a single result in CommonPrefixes. Objects and folders in the subfolders are not listed.

 For example, a bucket contains three objects: fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If Prefix is set to fun/, the three objects are returned. If Prefix is set to fun/ and Delimiter is set to a forward slash \(/\), fun/test.jpg and fun/movie/are returned.

 **Note:**

-   The parameter value must be smaller than 1,024 bytes in length.
-   If you specify a prefix to query objects, the names of the returned objects still contain the prefix.

 Default value: none |
|Encoding-type|String|No|The encoding type of the object name in the response. Default value: none

 Valid value: url

 **Note:** The Delimiter, Marker, Prefix, NextMarker, and Key values are UTF-8 encoded. If Delimiter, Marker, Prefix, NextMarker, and object names contain control characters that are not supported by the XML 1.0 standard, you can specify Encoding-type to encode Delimiter, Marker, Prefix, NextMarker, and Key in the returned results. |

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|Contents|Container|The container that stores the returned object metadata. Parent nodes: ListBucketResult |
|CommonPrefixes|String|If the Delimiter parameter is specified in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent nodes: ListBucketResult |
|Delimiter|String|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string from the prefix and the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent nodes: ListBucketResult |
|EncodingType|String|The encoding type of the object name in the response. If you specify Encoding-type in the request, Delimiter, Marker, Prefix, NextMarker, and Key are encoded in the returned results. Parent nodes: ListBucketResult |
|DisplayName|String|The name of the object owner. Parent nodes: ListBucketResult, Contents, and Owner |
|ETag|String|The entity tag \(ETag\). An ETag is created when the object is created to identify the content of an object. -   If an object is created using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created using other methods, the ETag value is the UUID of the object content.
-   The ETag value of the object can be used to check whether the object content is modified. However, we recommend that you do not use the ETag of an object as the MD5 hash of the object to verify data integrity.

 Parent nodes: ListBucketResult and Contents |
|ID|String|The user ID of the bucket owner. Parent nodes: ListBucketResult, Contents, and Owner |
|IsTruncated|Boolean|Indicates whether the returned results are truncated. Valid values: true and false

-   true indicates that not all of the results are returned this time.
-   false indicates that all of the results are returned this time.

 Parent nodes: ListBucketResult |
|Key|String|The key of the object. Parent nodes: ListBucketResult and Contents |
|LastModified|Date|The last modification time of the object. Parent nodes: ListBucketResult and Contents |
|ListBucketResult|Container|The container that stores the result of the GetBucket request. Child nodes: Name, Prefix, Marker, MaxKeys, Delimiter, IsTruncated, NextMarker, and Contents

 Parent nodes: none |
|Marker|String|The name of the object after which the listing begins. Parent nodes: ListBucketResult |
|MaxKeys|String|The maximum number of buckets returned each time. Parent nodes: ListBucketResult |
|Name|String|The name of the bucket. Parent nodes: ListBucketResult |
|Owner|Container|The container that stores the information about the bucket owner. Child nodes: DisplayName and ID

 Parent nodes: ListBucketResult |
|Prefix|String|The prefix that the returned object names must contain. Parent nodes: ListBucketResult |
|Size|String|The size of the returned object content. Unit: bytes. Parent nodes: ListBucketResult and Contents |
|StorageClass|String|The storage class of the object. Parent nodes: ListBucketResult and Contents |

## Examples

-   Sample request for simple GetBucket

    ```
    GET / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>oss.jpg</Key>
          <LastModified>2012-02-24T06:07:48.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   Sample request that has the Prefix parameter specified

    ```
    GET /? prefix=fun HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix>fun</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   Sample request that has the Prefix and Delimiter parameters specified

    ```
    GET /? prefix=fun/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix>fun/</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <CommonPrefixes>
          <Prefix>fun/movie/</Prefix>
    </CommonPrefixes>
    </ListBucketResult>
    ```

-   Sample request that has the Marker and Max-keys parameters specified

    In this example, the value of max-keys is set to 2, which indicates that the maximum number of objects to return is two.

    ```
    GET /? max-keys=2&marker=test1.txt HTTP/1.1
    Host: test-bucket-xxx.oss-cn-shenzhen.aliyuncs.com
    Accept-Encoding: identity
    Accept: */*
    Connection: keep-alive
    User-Agent: aliyun-sdk-python/2.11.0(Darwin/18.2.0/x86_64;3.4.1)
    date: Tue, 26 May 2020 08:39:48 GMT
    authorization: OSS LTAIB1VW9xxxxxxx:MmY11jLlO8UlAqjqHK3Ckp****
    ```

    Sample response

    The value of NextMarker in the response is the marker for next listing.

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 26 May 2020 08:39:48 GMT
    Content-Type: application/xml
    Content-Length: 1032
    Connection: keep-alive
    x-oss-request-id: 5ECCD5D4881816373582xxx
    x-oss-server-time: 3
    
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>test-bucket-xxx</Name>
      <Prefix></Prefix>
      <Marker>test1.txt</Marker>
      <MaxKeys>2</MaxKeys>
      <Delimiter></Delimiter>
      <EncodingType>url</EncodingType>
      <IsTruncated>true</IsTruncated>
      <NextMarker>test100.txt</NextMarker>
      <Contents>
        <Key>test10.txt</Key>
        <LastModified>2020-05-26T07:50:18.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
      <Contents>
        <Key>test100.txt</Key>
        <LastModified>2020-05-26T07:50:20.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
    </ListBucketResult>
    ```


## SDK

You can use the following SDKs for various programming languages to call GetBucket \(ListObjects\):

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/List objects.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/List objects.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/List objects.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/List objects.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/List objects.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/List objects.md)
-   [Android](/intl.en-US/SDK Reference/Android/Manage objects/List objects.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Overview.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Manage objects.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Manage objects.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to view the information about the bucket.|
|InvalidArgument|400|-   The error message returned because the value of max-keys is smaller than 0 or greater than 1000.
-   The error message returned because the length of the value of Prefix, Marker, or Delimiter is invalid. |

