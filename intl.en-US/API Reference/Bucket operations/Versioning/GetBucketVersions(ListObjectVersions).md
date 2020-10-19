# GetBucketVersions\(ListObjectVersions\)

You can call this operation to list all versions of all objects \(including delete markers\) in a bucket.

## Usage notes

When you perform the GetBucketVersions\(ListObjectVersions\) and GetBucket\(ListObjects\) operations on a versioned bucket, the following different results are returned:

-   For the GetBucket\(ListObjects\) operation, only the current versions of objects except delete markers in the bucket are returned.
-   For the GetBucketVersions\(ListObjectVersions\) operation, all versions of all objects in the bucket are returned.

    The versions of different objects are returned in alphabetic order. The versions of an object are returned in an order of time when they are created but not in alphabetic order of the version IDs.


## Request parameters

When you call the GetBucketVersions\(ListObjectVersions\) operation, you can specify the following parameters to filter the returned results: Prefix, Key-marker, Version-id-marker, Delimiter, and Max-keys.

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Delimiter|String|No|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.If you set Prefix to a folder name and Delimiter to a forward slash \(/\), only objects in the folder are returned. The names of the subfolders in the folder are returned in CommonPrefixes. However, the objects and folders in the subfolders are not returned.

Default value: null |
|Key-marker|String|The Key-marker must be specified if Version-id-marker is specified.|If this parameter is specified, objects whose names are alphabetically greater than the value of Key-marker are returned in alphabetic order. This parameter is specified together with Version-id-marker.The value of Key-marker must be smaller than 1,024 bytes in length.

Default value: null |
|Version-id-marker|String|No|If this parameter is specified, versions that are created for the object specified by Key-marker after the version specified by Version-id-marker are returned in an order of created time. The version specified by the parameter is not returned. By default, if this parameter is not specified, the results are returned from the latest version of the object whose name follows the object specified by Key-marker in alphabetic order. Default value: null

Valid values: version IDs |
|Max-keys|String|No|The maximum number of objects to return.If the number of returned objects exceeds the value of Max-keys, the response contains the NextKeyMarker and NextVersionIdMarker elements as the markers for the next list operation. Objects specified by NextKeyMarker and NextVersionIdMarker are included in the returned result.

Valid values: 1 to 1000

Default value: 100 |
|Prefix|String|No|The prefix that the returned object names must contain.If you specify a prefix in the request, the names of the returned objects contain the prefix.

The value of Prefix must be smaller than 1,024 bytes in length.

If you set Prefix to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

Default value: null |
|Encoding-type|String|No|The encoding type of the object name in the response.Default value: null

Valid value: URL

-   The values of Delimiter, Key-marker, Prefix, NextKeyMarker, and Key must be UTF-8 encoded.
-   If Delimiter, Key-marker, Prefix, NextKeyMarker, and Key contain control characters that are not supported by the XML 1.0 standard, you can specify Encoding-type to encode Delimiter, Key-marker, Prefix, NextKeyMarker, and Key in the response. |

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|ListVersionsResult|Container|The container used to store the results of the GetBucketVersions request.Child nodes: Name, Prefix, Marker, MaxKeys, Delimiter, IsTruncated, NextMarker, Version, and DeleteMarker

Parent nodes: none |
|CommonPrefixes|String|If the Delimiter parameter is specified in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.Parent node: ListVersionsResult |
|Delimiter|String|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.Parent node: ListVersionsResult |
|EncodingType|String|The encoding type of the object name in the response. If you specify EncodingType in the request, Delimiter, Marker, Prefix, NextMarker, and Key are encoded in the returned results.Parent node: ListVersionsResult |
|IsTruncated|String|Indicates whether the returned results are truncated. -   true: indicates that not all of the results are returned this time.
-   false: indicates that all of the results are returned this time.

Valid values: true and false

Parent node: ListVersionsResult |
|KeyMarker|String|Indicates the object from which the GetBucketVersions operation starts.Parent node: ListVersionsResult |
|VersionIdMarker|String|This parameter is returned with KeyMarker together to indicate the version from which the GetBucketVersion starts.Parent node: ListVersionsResult |
|NextKeyMarker|String|If not all results are returned for the request, the NextUploadMarker element is contained in the response to indicate the KeyMarker of the next request.Parent node: ListVersionsResult |
|NextVersionIdMarker|String|If not all results are returned for the request, the NextUploadMarker element is contained in the response to indicate the VersionIdMarker of the next request.Parent node: ListVersionsResult |
|MaxKeys|String|The maximum number of returned objects in the response.Parent node: ListVersionsResult |
|Name|String|The bucket name.Parent node: ListVersionsResult |
|Owner|Container|The container that stores the information about the bucket owner.Parent node: ListVersionsResult |
|Prefix|String|The prefix that the names of returned objects must contain.Parent node: ListVersionsResult |
|Version|Container|The container that stores the versions of objects except for delete markers.Parent node: ListVersionsResult |
|DeleteMarker|Container|The container that stores delete markers.Parent node: ListVersionsResult |
|ETag|String|The entity tag \(ETag\). An ETag is generated when an object is created to identify the content of the object. -   If an object is created by using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created by using other methods, the ETag value is the UUID of the object content.

**Note:** The ETag value of an object can be used to check whether the object content changes. However, we recommend that you do not use the ETag value of an object as the MD5 hash of the object to verify data integrity.

Parent node: ListVersionsResult.Version |
|Key|String|The name of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|LastModified|Date|The last modified time of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|VersionId|String|The version ID of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|IsLatest|String|Indicates whether the version is the current version.Valid values:

-   true: the version is the current version.
-   false: the version is a previous version.

Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|Size|String|The size of the returned object. Unit: bytes.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|StorageClass|String|The storage class of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|DisplayName|String|The name of the object owner.Parent nodes: ListVersionsResult.Version.Owner and ListVersionsResult.DeleteMarker.Owner |
|ID|String|The user ID of the bucket owner.Parent nodes: ListVersionsResult.Version.Owner and ListVersionsResult.DeleteMarker.Owner |

## Examples

-   Perform GetBucketVersions\(ListObjectVersions\) on an unversioned bucket

    Sample request

    ```
    GET /? versions HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76ov9cu:WFx4****+e7Rc0jawCsh7hlk****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    Content-Type: application/xml
    Content-Length: 1262
    Connection: keep-alive
    Date: Thu, Tue, 09 Apr 2019 07:27:48 GMT
    Server: AliyunOSS
    x-cos-request-id: 534B371674E88A4D8906****
    
    <ListVersionsResult>
        <Name>examplebucket-1250000000</Name>
        <Prefix/>
        <KeyMarker/>
        <VersionIdMarker/>
        <MaxKeys>1000</MaxKeys>
        <IsTruncated>false</IsTruncated>
        <Version>
            <Key>example-object-1.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-5T12:03:10.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B669CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-2.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-9T12:03:09.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B002CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-3.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-10T12:03:08.000Z</LastModified>
            <ETag>4B3F1A2E053D763E1B002CC607C5AGTRF****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```

-   Perform GetBucketVersions\(ListObjectVersions\) on a versioned bucket

    In this example, two objects named example and pic.jpg are stored in a bucket named oss example. The object named example has the following three versions in the order of the created time: 111222, 000123, and 222333, in which 000123 is a delete marker. The object named pic.jpg has only one version whose ID is 232323.

    If you set Key-marker to example and Version-id-marker to 111222, the following three versions are sequentially returned: 000123 of example, 222333 of example, and 232323 of pic.jpg

    Sample request

    ```
    GET /? versions&key-marker=example&version-id-marker=CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1**** HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76o****:WFx4kLpx+e7Rc0jawCsh7hlk****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC4974B7AEADE01700****
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListVersionsResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <KeyMarker>example</KeyMarker>
        <VersionIdMarker>CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1****</VersionIdMarker>
        <MaxKeys>100</MaxKeys>
        <Delimiter></Delimiter>
        <IsTruncated>false</IsTruncated>
        <DeleteMarker>
            <Key>example</Key>
            <VersionId>CAEQMxiBgICAof2D0BYiIDJhMGE3N2M1YTI1NDQzOGY5NTkyNTI3MGYyMzJm****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </DeleteMarker>
        <Version>
            <Key>example</Key>
            <VersionId>CAEQMxiBgMDNoP2D0BYiIDE3MWUxNzgxZDQxNTRiODI5OGYwZGMwNGY3MzZjN****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"250F8A0AE989679A22926A875F0A2****"</ETag>
            <Type>Normal</Type>
            <Size>93731</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>pic.jpg</Key>
            <VersionId>CAEQMxiBgMCZov2D0BYiIDY4MDllOTc2YmY5MjQxMzdiOGI3OTlhNTU0ODIx****</VersionId>
            <IsLatest>true</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"3663F7B0B9D3153F884C821E7CF4****"</ETag>
            <Type>Normal</Type>
            <Size>574768</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetBucketVersions \(ListObjectVersions\) operation:

-   [Java](/intl.en-US/SDK Reference/Java/Versioning/Versioning.md)
-   [Python](/intl.en-US/SDK Reference/Python/Versioning/Versioning.md)
-   [Go](/intl.en-US/SDK Reference/Go/Versioning/Versioning.md)
-   [C++](/intl.en-US/SDK Reference/C++/Versioning/Versioning.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Versioning/Versioning.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Versioning.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the bucket that you access does not exist. This error also returned when the bucket that you access cannot be created because the bucket name does not comply with the naming conventions.|
|AccessDenied|403|The error message returned because you are not authorized to access the bucket.|
|InvalidArgument|400|-   The error message returned because the value of Max-keys is smaller than 0 or greater than 1000.
-   The error message returned because the length of the value of Prefix, Marker, or Delimiter is invalid.
-   The error message returned because the value of Version-id-marker is invalid. |

