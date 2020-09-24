# SelectObject

You can call this operation to execute SQL statements on an object and return the execution result.

**Note:**

-   You must have read permissions on the object.
-   The 206 HTTP status code is returned if the operation is successful. If the SQL statement is not valid or cannot match any objects, the 400 HTTP status code is returned.
-   For more information about the functions of SelectObject, see [SelectObject](/intl.en-US/Developer Guide/Objects/Manage files/SelectObject.md) in OSS Developer Guide.

## Request structure

Request structure for CSV or JSON objects.

-   Request structure for CSV objects

    ```
    POST /object? x-oss-process=csv/select HTTP/1.1 
    HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: time GMT
    Content-Length: ContentLength
    Content-MD5: MD5Value 
    Authorization: Signature
    <? xml  version="1.0"  encoding="UTF-8"? >
    <SelectRequest>    
        <Expression>Base64-encoded SQL statement. Example: c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=</Expression>
        <InputSerialization>
            <CompressionType>None|GZIP</CompressionType>
            <CSV>            
                <FileHeaderInfo>
                    NONE|IGNORE|USE
                </FileHeaderInfo>
                <RecordDelimiter>Base64-encoded character</RecordDelimiter>
                <FieldDelimiter>Base64-encoded character</FieldDelimiter>
                <QuoteCharacter>Base64-encoded character</QuoteCharacter>
                <CommentCharacter>Base64-encoded character</CommentCharacter>
                <Range>line-range=start-end|split-range=start-end</Range>
                <AllowQuotedRecordDelimiter>true|false</AllowQuotedRecordDelimiter>
            </CSV>   
            </InputSerialization>
            <OutputSerialization>
                 <CSV>
                 <RecordDelimiter>Base64-encoded character</RecordDelimiter>
                 <FieldDelimiter>Base64-encoded character</FieldDelimiter>
    
                </CSV>
                <KeepAllColumns>false|true</KeepAllColumns>
                <OutputRawData>false|true</OutputRawData>
                <EnablePayloadCrc>true</EnablePayloadCrc>
                <OutputHeader>false</OutputHeader>
           </OutputSerialization>
         <Options>
            <SkipPartialDataRecord>false</SkipPartialDataRecord>
            <MaxSkippedRecordsAllowed>
            max allowed number of records skipped
            </MaxSkippedRecordsAllowed>
        </Options>
    </SelectRequest>
    ```

-   Request structure for JSON objects

    ```
    POST /object? x-oss-process=json/select HTTP/1.1 
    HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
    Date: time GMT
    Content-Length: ContentLength
    Content-MD5: MD5Value 
    Authorization: Signature
    <? xml  version="1.0"  encoding="UTF-8"? >
    <SelectRequest>    
        <Expression>
            Base64-encoded SQL statement. Example: c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=
        </Expression>
        <InputSerialization>
            <CompressionType>None|GZIP</CompressionType>
            <JSON>
                <Type>DOCUMENT|LINES</Type>
                <Range>
                line-range=start-end|split-range=start-end
                </Range>
                <ParseJsonNumberAsString> true|false
                </ParseJsonNumberAsString>
            </JSON>
        </InputSerialization>
        <OutputSerialization>
            <JSON>
                <RecordDelimiter>
                    Base64 of record delimiter
                </RecordDelimiter>
            </JSON>
            <OutputRawData>false|true</OutputRawData>
                     <EnablePayloadCrc>true</EnablePayloadCrc>
        </OutputSerialization>
        <Options>
            <SkipPartialDataRecord>
                false|true
            </SkipPartialDataRecord>
            <MaxSkippedRecordsAllowed>
                max allowed number of records skipped
               </MaxSkippedRecordsAllowed>
            </Options>
    </SelectRequest>
    ```


## Request elements

|Element|Type|Description|
|:------|:---|:----------|
|SelectRequest|Container|The container that stores the SelectObject request.

 Child nodes: Expression, InputSerialization, and OutputSerialization

 Parent nodes: none |
|Expression|String|The Base64-encoded SQL statement.

 Child nodes: none

 Parent nodes: SelectRequest |
|InputSerialization|Container|Optional. This element specifies the input serialization parameters.

 Child nodes: CompressionType, CSV, and JSON

 Parent nodes: SelectRequest |
|OutputSerialization|Container|Optional. This element specifies the output serialization parameters.

 Child nodes: CSV, JSON, and OutputRawData

 Parent nodes: SelectRequest |
|CSV\(InputSerialization\)|Container|Optional. This element specifies the input serialization parameters when the CSV object is queried.

 Child nodes: FileHeaderInfo, RecordDelimiter, FieldDelimiter, QuoteCharacter, CommentCharacter, and Range

 Parent nodes: InputSerialization |
|CSV\(OutputSerialization\)|Container|Optional. This element specifies the output serialization parameters when the CSV object is queried.

 Child nodes: RecordDelimiter and FieldDelimiter

 Parent nodes: OutputSerialization |
|JSON\(InputSerialization\)|Container|Optional. This element specifies the input serialization parameters when the JSON object is queried.

 Child nodes: Type, Range, and ParseJsonNumberAsString |
|JSON\(OutputSerialization\)|Container|Optional. This element specifies the output serialization parameters when the JSON object is queried.

 Child nodes: RecordDelimiter |
|Type|Enumeration|The type of the input JSON object: DOCUMENT or LINES. |
|OutputRawData|Boolean \(false by default\)|Optional. This element specifies whether to export raw data.

 Child nodes: none

 Parent nodes: OutputSerialization

 **Note:**

-   If you specify OutputRawData in the request, OSS returns data accordingly.
-   If you do not specify OutputRawData in the request, OSS automatically selects a format and returns it in the response.
-   If you explicitly set OutputRawData to True and it takes a long time for the sent SQL statement to return data, the HTTP request may time out. |
|CompressionType|Enumeration|The compression type of the object. Valid value: None or GZIP.

 Child nodes: none

 Parent nodes: InputSerialization |
|FileHeaderInfo|Enumeration|Optional. This element specifies the header information of the CSV object.

 Valid values:

 -   Use: The CSV object contains header information. You can use the column name in the CSV object as the column name in the SelectObject operation.
-   Ignore:The CSV object contains header information. However, you cannot use the column name in the CSV object as the column name in the SelectObject operation.
-   None: The CSV object does not contain header information. This is the default value.

 Child nodes: none

 Parent nodes: CSV \(input\) |
|RecordDelimiter|String|Optional. This element specifies a Base64-encoded line break. Default value: `\n`. The value of this element before the value is encoded can be an ANSI value of up to two characters in length. For example, `\n` is used to indicate a line break in Java.

 Child nodes: none

 Parent nodes: CSV \(input and output\) and JSON \(output\) |
|FieldDelimiter|String|Optional. This element specifies a Base64-encoded delimiter used to separate columns in the CSV object. Default value: `,`. The value of this element before the value is encoded must be a one-character ANSI value. For example, `,` is used to indicate a comma in Java code.

 Child nodes: none

 Parent nodes: CSV \(input and output\) |
|QuoteCharacter|String|Optional. This element specifies a Base64-encoded quote character used in the CSV object. Default value: `\"`. In a CSV object, line breaks and column delimiters enclosed in quotation marks are processed as normal characters. The value of this element before the value is encoded must be a one-character ANSI value. For example, `\"` is used to indicate a quote character in Java code.

 Child nodes: none

 Parent nodes: CSV \(input\) |
|CommentCharacter|String|The comment character used in the CSV object. The value of this element must be Base64-encoded. This value is empty by default.|
|Range|String|Optional. This element specifies the query range. The following two query methods are supported:

**Note:** SelectMeta must be created for objects that are queried based on Range. For more information about SelectMeta, see [CreateSelectObjectMeta](#CreateSelectObjectMeta).

 -   Query by row: line-range=start-end. For example, line-range=10-20 indicates that data from row 10 to row 20 is scanned.
-   Query by split: split-range=start-end. For example, split-range=10-20 indicates that data from split 10 to split 20 is scanned.

 The start and end parameters are both inclusive. The two preceding parameters have the same format as that of the range parameter in range get.

 This parameter can be used only when the object is in CSV format or when the JSON type is LINES.

 Child nodes: none

 Parent nodes: CSV \(input\) and JSON \(output\) |
|KeepAllColumns|bool|Optional. A value of true indicates that all columns in the CSV object are included in the response. The default value is false. However, only columns included in the SELECT clause have values. The columns in the response are sorted in ascending order of the column numbers. Example:

 `select _5, _1 from ossobject.`

 If you set KeepAllColumns to true and six columns are included in the CSV object, the following result is returned for the preceding SELECT clause:

 Value of 1st column,,,,Value of 5th column,\\n

 Child nodes: none

 Parent nodes: OutputSerialization \(CSV\) |
|EnablePayloadCrc|bool|The CRC-32 value for verification of each frame. The client can calculate the CRC-32 value of each payload and compare it with the included CRC-32 value to verify data integrity.

 Child nodes: none

 Parent nodes: OutputSerialization |
|Options|Container|Other optional parameters.

 Child nodes: SkipPartialDataRecord and MaxSkippedRecordsAllowed

 Parent nodes: SelectRequest |
|OutputHeader|bool|A value of true indicates that the header information of the CSV object is included in the beginning of the response.

 Default value: false.

 Child nodes: none

 Parent nodes: OutputSerialization |
|SkipPartialDataRecord|bool|Specifies whether to ignore rows where data is missing. If this parameter is set to false, OSS processes the row data as null without reporting errors. If this parameter is set to true, rows without data are skipped. If the number of skipped rows has exceeded the maximum allowed number, OSS reports an error and stops processing the data.

 Default value: false.

 Child nodes: none

 Parent nodes: Options |
|MaxSkippedRecordsAllowed|Int|The maximum allowed number of skipped rows. If a row does not match the type specified in the SQL statement, or if one or more columns in a row are missing and the value of SkipPartialDataRecord is True, the rows are skipped. If the number of skipped rows has exceeded the value of this parameter, OSS reports an error and stops processing the data.

 **Note:** If a row in a CSV object is not properly formatted, OSS stops processing the data and reports an error because this format error may result in incorrect resolution to the CSV object. For example, a column in the row includes continual odd numbered quote characters. This parameter can be used to adjust the tolerance for irregular data but cannot be applied to invalid CSV objects.

 Default value: 0

 Child nodes: none

 Parent nodes: Options |
|ParseJsonNumberAsString|bool|Parses integers and floating-point numbers in a JSON object into strings. The precision of floating-point numbers in a JSON object degrades when the numbers are parsed. Therefore, we recommend that you set the value of this parameter to true if you want to retain the raw data. To use the parsed numbers in calculations, you can use the CAST function in SQL to convert the parsed data into the required type such as int, double, or decimal.

 Default value: false

 Child nodes: none

 Parent nodes: JSON |
|AllowQuotedRecordDelimiter|bool|Specifies whether the CSV object contains line breaks in quotation marks \("\). For example, the value of a column is "abc\\ndef" and \\n is a line break, you must set this value to true. If this parameter is set to false, SelectObject allows you to specify a range in the request header for more efficient multipart query.

 Default value: true

 Child nodes: none

 Parent nodes: InputSerialization |

## Response body

-   If the HTTP status code included in the response is 4xx, the request has not passed the SQL syntax check, or the target object-related errors occur. In this case, the body format of the returned error message is the same as that of the error message returned for a GetObject request.
-   If the HTTP status code included in the response is 5xx, internal server errors occur. In this case, the body format of the returned error message is the same as that of the error message returned for a GetObject request.
-   HTTP status code 206 is returned when the operation is successful. In this case:
    -   If the value of header x-oss-select-output-raw is true, the object data \(not including frame-based data\) was successfully returned. The client can obtain the data in the same manner as the GetObject operation.
    -   If the value of x-oss-select-output-raw is false, the result is returned as frames.
-   The frames are in the format of `Version|Frame-Type | Payload Length | Header Checksum | Payload | Payload Checksum<1 byte><--3 bytes--><---4 bytes----><-------4 bytes--><variable><----4bytes---------->`.

    **Note:** The Checksum is CRC-32 in frames. All integers in a frame are big-endian. The value of Version is 1.


## Frame Type

The following table describes three frame types supported by SelectObject.

|Frame type|Value|Payload format|Description|
|----------|-----|--------------|-----------|
|Data Frame|8388609|offset \| data<-8 bytes\><---variable-\>|The data returned for the SelectObject request. The offset parameter is an 8-bit integer that indicates the current scanning location \(the offset from the file header\) and is used to report the progress of the operation.|
|Continuous Frame|8388612|offset<----8 bytes--\>|The frame used to report the progress of an operation and keep an HTTP connection. If no data is returned for a query request within five seconds, a continuous frame is returned.|
|End Frame|8388613|offset \| total scanned bytes \| http status code \| error message<--8bytes-\><--8bytes---------\><----4 bytes--------\><-variable------\>|The frame used to return the final status of an operation, including the number of scanned bytes and the possible error messages. -   The offset parameter indicates the final location offset after scanning.
-   The total scanned bytes parameter indicates the size of the scanned data.
-   The http status code parameter indicates the final status of the operation.

**Note:** SelectObject is a stream operation. Only the first data block is processed when the response header is sent. If the first data block matches the SQL statement, the HTTP status code in the response header is 206, which indicates that the operation was successful. However, the final status code may not be 206 because the subsequent data blocks may be invalid. The status code in the response header cannot be modified. Therefore, HTTP status code is included in the end frame to indicate the final status of the operation. The client uses the status code included in the end frame to determine whether the operation is successful.

-   The error message parameter includes error messages, including the number of each skipped row and the total quantity of skipped rows.

**Note:** The format of error messages included in an end frame is `ErrorCodes.DetailMessage`. The ErrorCodes section contains one or more error codes separated with commas \(,\). ErrorCodes and DetailMessage are separated with a period \(.\). |

## Sample requests

Sample requests for CSV or JSON objects.

-   Sample requests for CSV objects

    ```
    POST /oss-select/bigcsv_normal.csv? x-oss-process=csv%2Fselect HTTP/1.1
    Date: Fri, 25 May 2018 22:11:39 GMT
    Content-Type:
    Authorization: OSS LTAIJPLocA0fD:FC/9JRbBGRw4o2QqdaL246Px****
    User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
    Content-Length: 748
    Expect: 100-continue
    Connection: keep-alive
    Host: host name
    <? xml version="1.0"? >
    <SelectRequest>
        <Expression>c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=
        </Expression>
        <InputSerialization>
            <Compression>None</Compression>
            <CSV>
                <FileHeaderInfo>Ignore</FileHeaderInfo>
                <RecordDelimiter>Cg==</RecordDelimiter>
                <FieldDelimiter>LA==</FieldDelimiter>
                <QuoteCharacter>Ig==</QuoteCharacter>
                <CommentCharacter>Iw==</CommentCharacter/>
            </CSV>
        </InputSerialization>
        <OutputSerialization>
            <CSV>
                <RecordDelimiter>Cg==</RecordDelimiter>
                <FieldDelimiter>LA==</FieldDelimiter>
                <QuoteCharacter>Ig==</QuoteCharacter>            
            </CSV>
            <KeepAllColumns>false</KeepAllColumns>
                <OutputRawData>false</OutputRawData>
        </OutputSerialization>
    </SelectRequest>
    ```

-   Sample requests for JSON objects

    ```
    POST /oss-select/sample_json.json? x-oss-process=json%2Fselect HTTP/1.1
    Host: host name
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Darwin/16.7.0/x86_64;3.5.4)
    Accept: */*
    Connection: keep-alive
    date: Mon, 10 Dec 2018 18:28:11 GMT
    authorization: OSS AccessKeySignature
    Content-Length: 317
    <SelectRequest>
        <Expression>c2VsZWN0ICogZnJvbSBvc3NvYmplY3Qub2JqZWN0c1sqXSB3aGVyZSBwYXJ0eSA9ICdEZW1vY3JhdCc=
        </Expression>
        <InputSerialization>
        <JSON>
            <Type>DOCUMENT</Type>
        </JSON>
        </InputSerialization>
        <OutputSerialization>
        <JSON>
            <RecordDelimiter>LA==</RecordDelimiter>
        </JSON>
        </OutputSerialization>
        <Options />
    </SelectRequest>
    ```


## Regular expressions in an SQL statement

The regular expression in an SQL statement is `SELECT select-list from table where_opt limit_opt`.

**Note:** SELECT, OSSOBJECT, and WHERE are keywords that cannot be modified.

```
select_list: column name
            | column index (Example: _1, _2. column index applies only to CSV objects)
            | json path (Example: s.contacts.firstname. json path applies only to JSON objects)
            | function(column index | column name)
            | function(json_path) (Applies only to JSON objects)
            | select_list AS alias
```

**Note:** The following functions are supported: AVG, SUM, MAX, MIN, COUNT, and CAST \(type conversion function\). You can use only the wildcard \(\*\) after COUNT.

```
table: OSSOBJECT

      | OSSOBJECT json_path (Applies only to JSON objects)

For CSV objects, table must be OSSOBJECT. For JSON objects (including objects of the DOCUMENT and LINES type), you can specify json_path after OSSOBJECT.

json_path: ['string '] (The quotation marks around string can be deleted if the string does not include a space or a wildcard (*). In this case, [string] is equivalent to 'string'.)

          | [n] (Used to indicate the nth element in an array. The value of n is counted from 0.)

          | [*] (Used to indicate any child element in an array or object.)

          | .'string ' (The quotation marks around string can be deleted if the string does not include a space or a wildcard (*).)

          | json_path jsonpath (You can concatenate multiple elements in a json path such as [n].property1.attributes[*].)
```

```
Where_opt:
| WHERE expr
expr:
| literal value
| column name
| column index
| json path (Applies only to JSON objects)
| expr op expr
| expr OR expr
| expr AND expr
| expr IS NULL
| expr IS NOT NULL
| (column name | column index | json path) IN (value1, value2,...)
| (column name | column index | json path) NOT in (value1, value2,...)
| (column name | column index | json path) between value1 and value2
| NOT (expr)
| expr op expr
| (expr)
| cast (column index |column name | json path | literal as INT|DOUBLE|)
```

-   op: includes the following operators: `>`, `<`, `>=`, `<=`, `! =`, `=`, `,`, `LIKE`, `+`, `-`, `*`, `/`, `%`, and `||`.
-   cast: You can use the CAST function to convert data in a column from one type into another.
-   The combination of an aggregation function and limit: `Select avg(cast(_1 as int)) from ossobject limit 100`. The preceding statement calculates the average value of the first columns in the first 100 rows. This function differs from the statement supported by MySQL because only a single row is returned for an aggregation function in SelectObject operations. This makes it unnecessary to limit the output data size. The operation for limit is performed before the aggregation operation when you call SelectObject.

## Limits on SQL statements

Limits on SQL statements:

-   Only text objects encoded in UTF-8 and UTF-8 text objects compressed in the GZIP format are supported. The deflate format is not supported for GZIP objects.
-   Only a single object can be queried when you use an SQL statement. The following clauses are not supported: JOIN, ORDER BY, GROUP BY, and HAVING clauses.
-   A WHERE clause cannot include aggregation conditions. For example, the following clause is not allowed: WHERE max\(cast\(age as int\)\) \> 100.
-   A maximum of 1,000 columns can be specified for an SQL statement. The column name in an SQL statement can be a maximum of 1,024 bytes in length.
-   A maximum of five wildcards \(%\) are supported in a LIKE clause. The percent sign \(%\) and the asterisk \(\*\) are wildcards that represent zero or more characters. The ESCAPE keyword is supported in SQL LIKE clauses, and is used to convert special characters such as percent signs \(%\), asterisks \(\*\), and question marks \(?\) into normal strings.
-   A maximum of 1,024 constants are supported in an IN clause.
-   The Projection after SELECT can be a column name, a CSV column index \(such as \_1 and \_2\), an aggregation function, or a CAST function. Other expressions are not supported such as select \_1 + \_2 from ossobject.
-   The maximum column size and row size for a CSV object are 256 KB.
-   json path after FROM supports a JSON node with a maximum size of 512 KB. The path can contain up to 10 levels, and an array can contain a maximum of 5,000 elements. The fields after SELECT and WHERE must be from the nodes corresponding to json path after FROM.
-   In SQL statements for a JSON object, the SELECT or WHERE expressions cannot include array wildcards \(\[\*\]\). Array wildcards \(\[\*\]\) can be included only in json path after FROM. For example, select \* from ossobject.contacts\[\*\] instead of select s.contacts\[\*\] from ossobject s is supported.
-   The maximum size of an SQL statement is 16 KB. A maximum of 20 expressions can be added after WHERE. Each statement supports up to 10 levels and 100 aggregation operations.

## Data error handling

The following section lists common methods to handle data errors.

-   Some columns are missing in some rows in a CSV object.

    If the value of SkipPartialDataRecord is not specified or is set to False, OSS calculates the expressions in the SQL statement by processing the values of the missing columns as null.

    If the value of SkipPartialDataRecord is set to True, OSS ignores the rows in which some columns are missing. In this case, if the value of MaxSkippedRecordsAllowed is not specified or is set to a value smaller than the number of skipped rows, OSS reports an error by sending HTTP status code 400 or including HTTP status code 400 in the end frame.

    Assume that the SQL statement is `select _1, _3 from ossobject` and that the data in a row of the CSV object is "John, Company A".

    -   If the value of SkipPartialDataRecord is set to False, "John,\\n" is returned.
    -   If the value of SkipPartialDataRecord is set to True, this row is skipped.
-   Some keys are missing in a JSON object.

    Some JSON objects may exclude the keys specified in the SQL statement.

    -   If the value of SkipPartialDataRecord is not specified or is set to False, OSS calculates the expressions in the SQL statement by processing the missing keys as null.
    -   If the value of SkipPartialDataRecord is True, OSS skips the data in the JSON node. In this case, if the value of MaxSkippedRecordsAllowed is not specified or is set to a value smaller than the number of skipped rows, OSS reports an error by sending HTTP status code 400 or including HTTP status code 400 in the end frame.
    Assume that the SQL statement is `select s.firstName, s.lastName , s.age from ossobject.contacts[*] s`, and that the value of a JSON node is \{"firstName":"John", "lastName":"Smith"\}.

    -   If the value of SkipPartialDataRecord is not specified or is set to False, \{"firstName":"John", "lastName":"Smith"\} is returned.
    -   If the value of SkipPartialDataRecord is set to True, this row is skipped.
    **Note:** For keys in the returned data of a request for JSON objects, the output JSON objects can only be LINES. The Key value in the output result is determined based on the following rules.

    -   Assume that the SQL statement is `select * from ossobject…`. If \* corresponds to a JSON object \(\{...\}\), it is returned. If \* corresponds to a string or an array, it is returned as a DummyKey \_1.

        When the data is \{"Age":5\}`select * from ossobject.Age s where s = 5`, \{"\_1":5\} is returned because 5 that corresponds to \* is not a JSON object. When the SQL statement is `select * from ossobject s where s.Age = 5`, \{"Age":5\} corresponding to \* is returned.

    -   If the SQL statement does not use select \* but specifies columns, the returned content is in the `{"{Column 1}": Value, "{Column 2}": Value...}` format. \{Column n\} can be generated in the following ways:

        -   If the alias of the column is specified in the SELECT clause, the alias applies.
        -   If the column name is the key of a JSON object, this key is used as the output key value.
        -   If the column is an element of a JSON array or an aggregate function, prefix the column name with the serial number \(starting from 1\) and underscore \(\_\) as the output key value.
        Assume that the data is \{"contacts":\{"Age":35, "Children":\["child1", "child2", "child3"\]\}\}:

        -   When the SQL statement is `select s.contacts.Age, s.contacts.Children[0] from ossobjects`, Age is the key of the input JSON object, and Children\[0\] indicates the first element of Children of the array type and is the second column in the output content. \{"Age":35, "\_2":"child1"\} is returned.
        -   When the SQL statement is `select max(cast(s.Age as int)) from ossobject.contacts s` and the selected column is an aggregate function, the column is prefixed with \_1 and its serial number in the output. \{"\_1":35\} is returned.
        -   When the alias of the column is specified in SQL statement `select s.contacts.Age, s.contacts.Children[0] as firstChild from ossobject`, \{"Age":35, "firstChild":"child1"\} is returned.
    -   Keys that match the JSON objects and SQL statements are case-sensitive. For example, "select s. Age" and "select s. age" are different.
-   The data type of some columns in a CSV object does not match the SQL statement.

    If the data type of a row in a CSV object does not match the type specified in the SQL statement, the row is skipped. If the number of skipped rows exceeds the value of MaxSkippedRecordsAllowed, OSS stops processing data and returns the 400 HTTP status code.

    Assume that the SQL statement is `select _1, _3 from ossobject where _3 > 5`. If the value of a row in the CSV object is `John, Company A, To be hired`, this row is ignored because the third column in the row is not of the integer type.

-   The data type of some keys in a JSON object does not match the SQL statement.

    Assume that the SQL statement is `select s.name from ossobject s where s.aliren_age > 5`. If the value of a JSON node is `{"Name":"John", "aliren_age":To be hired}`, this node is skipped.


## CreateSelectObjectMeta

You can call this operation to obtain information about the requested CSV object such as the total number of rows, the total number of columns in the CSV object, and the number of splits. If the information does not exist in the object, the entire CSV object is scanned to analyze and record the preceding information. The information obtained in the first API operation call is used when the operation is called again. This way, the entire CSV object does not need to be scanned again.

**Note:**

-   You must have the write permissions on the target object before you can perform the CreateSelectObjectMeta operation.
-   The 200 HTTP status code is returned if the operation is successful. If the target object is not a valid CSV or JSON LINES object, or the specified delimiter does not match any CSV objects, the 400 HTTP status code 400 is returned.

-   Request structure
    -   Request structure for CSV objects

        ```
        POST  /samplecsv? x-oss-process=csv/meta
        <CsvMetaRequest>
            <InputSerialization>
                <CompressionType>None</CompressionType>
                <CSV>
                    <RecordDelimiter>Base64-encoded character</RecordDelimiter>
                    <FieldDelimiter>Base64-encoded character</FieldDelimiter>
                    <QuoteCharacter>Base64-encoded character</QuoteCharacter>
                </CSV>
            </InputSerialization>
            <OverwriteIfExists>false|true</OverwriteIfExists>
        </CsvMetaRequest>
        ```

    -   Request structure for JSON objects

        ```
        POST  /samplecsv? x-oss-process=json/meta
        <JsonMetaRequest>
            <InputSerialization>
                <CompressionType>None</CompressionType>
                <JSON>
                    <Type>LINES</Type>
                </JSON>
            </InputSerialization>
            <OverwriteIfExists>false|true</OverwriteIfExists>
        </JsonMetaRequest>
        ```

-   Request elements

    |Element|Type|Description|
    |:------|----|:----------|
    |CsvMetaRequest|Container|The container that stores the CreateSelectObjectMeta request for CSV objects.

 Child nodes: InputSerialization

 Parent nodes: none |
    |JsonMetaRequest|Container|The container that stores the CreateSelectObjectMeta request for JSON objects.

 Child nodes: InputSerialization

 Parent nodes: none |
    |InputSerialization|Container|Optional. This element specifies the input serialization parameters.

 Child nodes: CompressionType, CSV, and JSON

 Parent nodes: CsvMetaRequest and JsonMetaRequest |
    |OverwriteIfExists|bool|Optional. This element specifies whether to recalculate the SelectMeta and overwrite the existing data. A value of false indicates that the result is directly returned if the SelectMeta already exists. The default value is false.

 Child nodes: none

 Parent nodes: CsvMetaRequest and JsonMetaRequest |
    |CompressionType|Enumeration|Optional. This element specifies the compression type of the object. Only None is supported.

 Child nodes: none

 Parent nodes: InputSerialization |
    |RecordDelimiter|String|Optional. This element specifies a Base64-encoded line break used to separate rows in the CSV object. The default value is \\n. The value of this element before the value is encoded can be an ANSI value of up to two characters in length. For example, \\n is used to indicate a line break in Java code.

 Child nodes: none

 Parent nodes: CSV |
    |FieldDelimiter|String|Optional. This element specifies a Base64-encoded delimiter used to separate columns in the CSV object. Default value: `,`.

 The value of this element before being encoded must be a one-character ANSI value. For example, `,` is used to indicate a comma in Java code.

 Child nodes: none

 Parent nodes: CSV \(input and output\) |
    |QuoteCharacter|String|Optional. This element specifies a Base64-encoded quote character used in the CSV object. Default value: \\". In a CSV object, line breaks and column delimiters enclosed in quotation marks are processed as normal characters. The value of this element before the value is encoded can be an ANSI value of up to two characters in length. For example, \\" is used to indicate a quote character in Java code.

 Child nodes: none

 Parent nodes: CSV \(input\) |
    |CSV|Container|The format of the input CSV object.

 Child nodes: RecordDelimiter, FieldDelimiter, and QuoteCharacter

 Parent nodes: InputSerialization |
    |JSON|Container|The format of the input JSON object.

 Child nodes: Type

 Parent nodes: InputSerialization |
    |Type|Enumeration|The type of the JSON object.

 Valid values: LINES |

-   Response body

    Similar to SelectObject, the results for CreateSelectObjectMeta are also returned as frames.

    |Frame type|Value|Payload format|Description|
    |:---------|:----|:-------------|:----------|
    |Meta End Frame \(CSV\)|8388614|offset \| total scanned bytes \| status\| splits count \| rows count \| columns count \| error message

 <-8 bytes\><------8 bytes------\><--4bytes\><--4 bytes--\><--8 bytes\><--4 bytes---\><variable size\>

|Reports the final status of the CreateSelectObjectMeta operation.

     -   offset: an 8-bit integer, which indicates the offset when the scanning is complete.
    -   total scanned bytes: an 8-bit integer, which indicates the size of the scanned data.
    -   status: a 4-bit integer, which indicates the final status of the operation.
    -   splits\_count: a 4-bit integer, which indicates the total number of splits.
    -   rows\_count: an 8-bit integer, which indicates the total number of rows.
    -   cols\_count: a 4-bit integer, which indicates the total number of columns.
    -   error\_message: detailed error messages. If no errors occur, the value of this parameter is null. |
    |Meta End Frame \(JSON\)|8388615|offset \| total scanned bytes \| status\| splits count \| rows count \| error message

 <-8 bytes\><------8 bytes------\><--4bytes\><--4 bytes--\><--8 bytes\><variable size\>

|Reports the final status of the CreateSelectObjectMeta operation.

     -   offset: an 8-bit integer, which indicates the offset when the scanning is complete.
    -   total scanned bytes: an 8-bit integer, which indicates the size of the scanned data.
    -   status: a 4-bit integer, which indicates the final status of the operation.
    -   splits\_count: a 4-bit integer, which indicates the total number of splits.
    -   rows\_count: an 8-bit integer, which indicates the total number of rows.
    -   error\_message: detailed error messages. If no errors occur, the value of this parameter is null. |

-   Sample requests
    -   Sample requests for CSV objects

        ```
        POST /oss-select/bigcsv_normal.csv? x-oss-process=csv%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
        User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
        Content-Length: 309
        Expect: 100-continue
        Connection: keep-alive
        Host: Host
        <? xml version="1.0"? >
        <CsvMetaRequest>
            <InputSerialization>
                <CSV>
                    <RecordDelimiter>Cg==</RecordDelimiter>
                    <FieldDelimiter>LA==</FieldDelimiter>
                    <QuoteCharacter>Ig==</QuoteCharacter>
                </CSV>
            </InputSerialization>
            <OverwriteIfExisting>false</OverwriteIfExisting>
        </CsvMetaRequest>
        ```

    -   Sample requests for JSON objects

        ```
        POST /oss-select/sample.json? x-oss-process=json%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
        User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
        Content-Length: 309
        Expect: 100-continue
        Connection: keep-alive
        Host: Host
        <? xml version="1.0"? >
        <JsonMetaRequest>
            <InputSerialization>
                <JSON>
                    <Type>LINES</Type>
                </JSON>
            </InputSerialization>
            <OverwriteIfExisting>false</OverwriteIfExisting>
        </JsonMetaRequest>
        ```


## Supported time formats

You can convert any of the following listed formats to a timestamp without specifying the time format. For example, string cast­\('20121201' as timestamp\) is automatically parsed as a timestamp: December 1, 2012.

The following table describes the recognized time formats.

|Format|Description|
|:-----|:----------|
|YYYYMMDD|year month day|
|YYYY/MM/DD|year/month/day|
|DD/MM/YYYY/|day/month/year|
|YYYY-MM-DD|year-month-day|
|DD-MM-YY|day-month-year|
|DD.MM.YY|day. month.year|
|HH:MM:SS.mss|hour:minute:second. millisecond|
|HH:MM:SS|hour:minute:second|
|HH MM SS mss|hour minute second millisecond|
|HH.MM.SS.mss|hour. minute. second. millisecond|
|HHMM|hour minute|
|HHMMSSmss|hour minute second millisecond|
|YYYYMMDD HH:MM:SS.mss|year month day hour:minute:second. millisecond|
|YYYY/MM/DD HH:MM:SS.mss|year/month/day hour:minute:second. millisecond|
|DD/MM/YYYY HH:MM:SS.mss|day/month/year hour:minute:second. millisecond|
|YYYYMMDD HH:MM:SS|year month day hour:minute:second|
|YYYY/MM/DD HH:MM:SS|year/month/day hour:minute:second|
|DD/MM/YYYY HH:MM:SS|day/month/year hour:minute:second|
|YYYY-MM-DD HH:MM:SS.mss|year-month-day hour:minute:second. millisecond|
|DD-MM-YYYY HH:MM:SS.mss|day-month-year hour:minute:second. millisecond|
|YYYY-MM-DD HH:MM:SS|year-month-day hour:minute:second|
|YYYYMMDDTHH:MM:SS|year month day T hour:minute:second|
|YYYYMMDDTHH:MM:SS.mss|year month day T hour:minute:second. millisecond|
|DD-MM-YYYYTHH:MM:SS.mss|day-month-year T hour:minute:second. millisecond|
|DD-MM-YYYYTHH:MM:SS|day-month-year T hour:minute:second|
|YYYYMMDDTHHMM|year month day T hour minute|
|YYYYMMDDTHHMMSS|year month day T hour minute second|
|YYYYMMDDTHHMMSSMSS|year month day T hour minute second millisecond|
|ISO8601-0|year-month-day T hour:minute+hour:minute, or year-month-day T hour:minute-hour:minute

 "+" indicates that the time in the current time zone is ahead of standard UTC time. "-" indicates that the time in the current time zone is behind the standard UTC time. In this format, ISO 8601-0 can be used. |
|ISO8601-1|year-month-day T hour:minute+hour:minute, or year-month-day T hour:minute-hour:minute

 "+" indicates that the time in the current time zone is ahead of standard UTC time. "-" indicates that the time in the current time zone is behind the standard UTC time. In this format, ISO 8601-1 can be used. |
|CommonLog|Example: 28/Feb/2017:12:30:51 +0700|
|RFC822|Example: Tue, 28 Feb 2017 12:30:51 GMT|
|? D/? M/YY|day/month/year, in which the day and month can be one or two digits.|
|? D/? M/YY ? H:? M|day/month/year/hour:minute, in which the day, month, hour, and minute can be one or two digits.|
|? D/? M/YY ? H:? M:? S|day/month/year/hour:minute:second, in which the day, month, hour, minute, and second can be one or two digits.|

The formats in the following table are ambiguous. You must specify a time format when you use strings in these formats. For example, the cast\('20121201' as timestamp format 'YYYYDDMM'\) statement incorrectly parses string 20121201 as January 12, 2012.

|Format|Description|
|:-----|:----------|
|YYYYDDMM|year day month|
|YYYY/DD/MM|year/day/month|
|MM/DD/YYYY|month/day/year|
|YYYY-DD-MM|year-day-month|
|MM-DD-YYYY|month-day-year|
|MM.DD.YYYY|month. day.year|

## ErrorCode

SelectObject returns error codes in the following ways:

-   The HTTP status code is included in the response header, and the error code is included in the response body, which is the same as other OSS requests. Error codes returned in this manner indicate that input errors such as an invalid SQL statement or data errors occur.
-   The error code is included in the end frame of the response body. Error codes returned in this manner indicate that the data is incorrect or does not match the SQL statement. For example, a string exists in a column for which the type is set to integer in the SQL statement. In this case, part of data is processed and returned to the client, and the HTTP status code is 206.

Error codes such as InvalidCSVLine can be returned an HTTP status code in the response header or as a status code in the end frame based on the location of the error row within the CSV object.

|ErrorCode|Description|HTTP Status Code|Http Status Code in End Frame|
|---------|-----------|----------------|-----------------------------|
|InvalidSqlParameter|Invalid SQL parameter. The SQL statement in the request is null, the SQL statement size has exceeded the limit, or the SQL statement is not Base64-encoded.

|400|None|
|InvalidInputFieldDelimiter|The error message returned because the input CSV object contains invalid column delimiters. The parameter is not Base64-encoded, or the size of the parameter is greater than 1 byte after being decoded.

|400|None|
|InvalidInputRecordDelimiter|The error message returned because the input CSV object contains invalid row delimiters. The parameter is not Base64-encoded, or the size of the parameter is greater than 2 bytes after being decoded.|400|None|
|InvalidInputQuote|The error message returned because the input CSV object contains invalid quote characters. The parameter is not Base64-encoded, or the size of the parameter is greater than 1 byte after being decoded.|400|None|
|InvalidOutputFieldDelimiter|The error message returned because the output CSV object contains Invalid column delimiters. The parameter is not Base64-encoded, or the size of the parameter is greater than 1 byte after being decoded.|400|None|
|InvalidOutputRecordDelimiter|The error message returned because the output CSV object contains Invalid column delimiters. The parameter is not Base64-encoded, or the size of the parameter is greater than 2 bytes after being decoded.|400|None|
|UnsupportedCompressionFormat|The error message returned because the value of the Compression parameter is not NONE or GZIP \(case-insensitive\).|400|None|
|InvalidCommentCharacter|The error message returned because the CSV object contains invalid comment characters. The parameter is not Base64-encoded, or the size of the parameter is greater than 1 byte after being decoded.|400|None|
|InvalidRange|The error message returned because the Range parameter is not prefixed with line-range= or split-range=, or the range value does not comply with the HTTP standard for Range.|400|None|
|DecompressFailure|The error message returned because the value of Compression is GZIP and the object cannot be decompressed.|400|None|
|InvalidMaxSkippedRecordsAllowed|The error message returned because the value of MaxSkippedRecordsAllowed is not an integer.|400|None|
|SelectCsvMetaUnavailable|The error message returned because the target object does not include CSV Meta when the Range parameter is specified. Call the CreateSelectObjectMeta operation first.|400|None|
|InvalidTextEncoding|The error message returned because the object is not UTF-8 encoded.|400|None|
|InvalidOSSSelectParameters|The error message returned because the EnablePayloadCrc and OutputRawData parameters are both set to True, which results in conflicts.|400|None|
|InternalError|The error message returned because an OSS system error has occurred.|500 or 206|None or 500|
|SqlSyntaxError|The error message returned because the syntax of the Base64-decoded SQL statement is invalid.|400|None|
|SqlExceedsMaxInCount|The error message returned because the number of values included in the SQL IN clause has exceeded 1,024.|400|None|
|SqlExceedsMaxColumnNameLength|The error message returned because the size of the column name has exceeded 1,024 bytes.|400|None|
|SqlInvalidColumnIndex|The error message returned because the column index in the SQL statement is smaller than 1 byte or greater than 1,000 bytes in length.|400|None|
|SqlAggregationOnNonNumericType|The error message returned because an aggregation function is used in a non-numeric column.|400|None|
|SqlInvalidAggregationOnTimestamp|The error message returned because the SUM or AVG aggregation function is used in the timestamp column.|400|None|
|SqlValueTypeOfInMustBeSame|The error message returned because values of different types are included in the SQL IN clause.|400|None|
|SqlInvalidEscapeChar|The error message returned because an invalid escape character such as a question mark \(?\), percent sign \(%\), or an asterisk \(\*\) was specified in the SQL LIKE clause.|400|None|
|SqlOnlyOneEscapeCharIsAllowed|The error message returned because the size of the escape character in the SQL LIKE clause is greater than 1 byte in length.|400|None|
|SqlNoCharAfterEscapeChar|The error message returned because no characters after the escape character in the SQL LIKE clause exist.|400|None|
|SqlInvalidLimitValue|The error message returned because the number after the SQL Limit clause is smaller than 1.|400|None|
|SqlExceedsMaxWildCardCount|The error message returned because the number of wildcards \(\*\) or \(%\) in the SQL LIKE clause has exceeded the limit.|400|None|
|SqlExceedsMaxConditionCount|The error message returned because the number of conditional expressions in the SQL WHERE clause has exceeded the limit.|400|None|
|SqlExceedsMaxConditionDepth|The error message returned because the depth of the conditional tree in the SQL WHERE clause has exceeded the limit.|400|None|
|SqlOneColumnCastToDifferentTypes|The error message returned because a column was converted into different types by including the CAST function in the SQL statement.|400|None|
|SqlOperationAppliedToDifferentTypes|The error message returned because an operator was used for two objects of different types in the SQL statement. For example, this error code is returned if col1 in \_col1 \> 3 is a string.|400|None|
|SqlInvalidColumnName|The error message returned because a column name used in the SQL statement was excluded in the header of the CSV object.|400|None|
|SqlNotSupportedTimestampFormat|The error message returned because the timestamp format specified in the SQL CAST clause is not supported.|400|None|
|SqlNotMatchTimestampFormat|The error message returned because the timestamp format specified in the SQL CAST clause does not match the timestamp string.|400|None|
|SqlInvalidTimestampValue|The error message returned because no timestamp formats are specified in the SQL CAST clause and the provided string cannot be converted to a timestamp.|400|None|
|SqlInvalidLikeOperand|The error message returned because column names or indexes are not specified in the SQL LIKE clause, the specified column is not of the string type, or the right column in the LIKE clause is of the string type.|400|None|
|SqlInvalidMixOfAggregationAndColumn|The error message returned because the SQL SELECT clause includes column names and indexes for both aggregation functions and non-aggregation functions.|400|None|
|SqlExceedsMaxAggregationCount|The error message returned because the number of aggregation functions included in the SQL SELECT clause has exceeded the limit.|400|None|
|SqlInvalidMixOfStarAndColumn|The error message returned because an asterisk \(\*\), a column name, and a column index are included in the same SQL statement.|400|None|
|SqlInvalidKeepAllColumnsWithAggregation|The error message returned because the SQL statement includes aggregation functions while the KeepAllColumns parameter is set to True.|400|None|
|SqlInvalidKeepAllColumnsWithDuplicateColumn|The error message returned because the SQL statement includes repeated column names or column indexes while the KeepAllColumns parameter is set to True.|400|None|
|SqlInvalidSqlAfterAnalysis|The error message returned because the SQL statement is not supported because the SQL statement is too complex to parse.|400|None|
|InvalidArithmeticOperand|The error message returned because the SQL statement contains arithmetic operations performed on non-numeric constants or columns.|400|None|
|SqlInvalidAndOperand|The error message returned because the expressions connected by the AND operator in the SQL statement are not of the Boolean type.|400|None|
|SqlInvalidOrOperand|The error message returned because the expressions connected by the OR operator in the SQL statement are not of the Boolean type.|400|None|
|SqlInvalidNotOperand|The error message returned because the expressions connected by the NOT operator in the SQL statement are not of the Boolean type.|400|None|
|SqlInvalidIsNullOperand|The error message returned because the SQL statement specifies the IS NULL operator-based operations performed on a constant.|400|None|
|SqlComparerOperandTypeMismatch|The error message returned because the SQL statement specifies the comparison operator-based operations performed on two objects of different types.|400|None|
|SqlInvalidConcatOperand|The error message returned because the SQL statement contains two constants connected by the concatenation operator \(\|\|\).|400|None|
|SqlUnsupportedSql|The error message returned because the SQL statement is too complex so that the size of the generated SQL plan has exceeded the limit.|400|None|
|HeaderInfoExceedsMaxSize|The error message returned because the size of the header information specified in the SQL statement has exceeded the limit.|400|None|
|OutputExceedsMaxSize|The error message returned because a row of output results has exceeded the size limit.|400|None|
|InvalidCsvLine|The error message returned because a row in the CSV object is invalid or has exceeded the size limit, or the number of skipped rows has exceeded the value of MaxSkippedRecordsAllowed.|206 or 400|400 or None|
|NegativeRowIndex|The error message returned because the value of the array index in the SQL statement is a negative number.|400|None|
|ExceedsMaxNestedColumnDepth|The error message returned because the number of nested levels of the JSON object in the SQL statement has exceeded the level limit.|400|None|
|NestedColumnNotSupportInCsv|The error message returned because the SQL statement contains nested columns that include periods \(.\) or arrays with brackets \(\[\]\), which are not supported for SQL statements of CSV objects.|400|None|
|TableRootNodeOnlySupportInJson|The error message returned because the root node path can be specified only after From ossobject in JSON objects.|400|None|
|JsonNodeExceedsMaxSize|The error message returned because the size of the root node in the JSON object has exceeded the limit.|400 or 206|None or 400|
|InvalidJsonData|The error message returned because the JSON data is formatted incorrectly.|400 or 206|None or 400|
|ExceedsMaxJsonArraySize|The error message returned because the number of elements in an array in the root node of the JSON object has exceeded the limit.|400 or 206|None or 400|
|WildCardNotAllowed|The error message returned because the wildcard \(\*\) cannot be used in SQL SELECT clauses or SQL WHERE clauses for the JSON object. For example, the following statement is not supported: `select s.a.b[*] from ossobject where a.c[*] > 0`.|400|None|
|JsonNodeExceedsMaxDepth|The error message returned because the depth of the root node of the JSON object has exceeded the limit.|400 or 206|None or 400|

