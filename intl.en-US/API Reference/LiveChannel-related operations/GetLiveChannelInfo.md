# GetLiveChannelInfo

Obtains the configuration information about a specified LiveChannel.

## Request syntax

```
GET /ChannelName?live HTTP/1.1
Date: GMT date
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
```

## Response element

|Element|Type|Description|
|:------|:---|:----------|
|LiveChannelConfiguration|Container|Specifies the container that stores the response to the GetLiveChannelInfo request.Sub-node: Description, Status, and Target

Parent node: None |
|Description|String|Specifies the description of the LiveChannel.Sub-node: None

Parent node: LiveChannelConfiguration |
|Status|Enumerated string|Indicates the status of the LiveChannel.Sub-node: None

Parent node: LiveChannelConfiguration

Valid value: enabled and disabled |
|Target|Container|Specifies the container used to store the settings for storing uploaded data.Sub-node: Type, FragDuration, FragCount, and PlaylistName

Parent node: LiveChannelConfiguration |
|Type|Enumerated string|Specifies the format that the uploaded data is stored as when its value is HLS.Sub-node: None

Parent-node: Target

Valid value: HLS |
|FragDuration|String|Specifies the duration \(in seconds\) of each ts file when the value of Type is HLS.Sub-node: None

Parent node: Target |
|FragCount|String|Specifies the number of ts files included in the m3u8 file when the value of Type is HLS.Sub-node: None

Parent node: Target |
|PlaylistName|String|Specifies the name of the m3u8 file generated when the value of Type is HLS.Sub-node: None

Parent node: Target |

## Detail analysis

The sub-nodes of Target, including FragDuration, FragCount, and PlaylistName, are returned only when the value of Type is HLS.

## Examples

Request example

```
GET /test-channel?live HTTP/1.1
Date: Thu, 25 Aug 2016 05:52:40 GMT
Host: test-bucket.oss-cn-hangzhou.aliyuncs.com
Authorization: OSS YJjHKOKWDWINLKXv:D6bDCRXKht58hin1BL83wxyGvl0=
```

Response example

```
HTTP/1.1 200
content-length: 475
server: AliyunOSS
connection: close
x-oss-request-id: 57BE87A8B92475920B002098
date: Thu, 25 Aug 2016 05:52:40 GMT
content-type: application/xml
<?xml version="1.0" encoding="UTF-8"?>
<LiveChannelConfiguration>
  <Description></Description>
  <Status>enabled</Status>
  <Target>
    <Type>HLS</Type>
    <FragDuration>2</FragDuration>
    <FragCount>3</FragCount>
    <PlaylistName>playlist.m3u8</PlaylistName>
  </Target>
</LiveChannelConfiguration>
```

