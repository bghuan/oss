# 基于OSS构建HLS流

OSS支持以RTMP协议推流音视频至存储空间（Bucket），并转储为HLS协议格式，同时提供了丰富的鉴权、授权机制实现更细颗粒度的音视频数据访问控制。

## 前提条件

已创建了存储空间。详情请参见[创建存储空间](/intl.zh-CN/快速入门/创建存储空间.md)。

## 基础操作

OSS支持使用RTMP推流协议上传H264格式的视频数据和AAC格式的音频数据，并通过访问PlayURL地址的方式获取音视频数据。

-   上传音视频数据
    1.  调用[PutLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)创建LiveChannel。

        调用该接口会返回RTMP推流地址PublishURL和播放地址PlayURL。您还可以使用SDK的方式创建LiveChannel，详情请参见[Python SDK 的 LiveChannel 常见操作]()。

    2.  通过PublishURL推流音视频文件。

        OSS将上传的音视频文件按HLS协议转储，即转储为一个m3u8格式的索引文件和多个ts格式的视频文件，详情请参见[RTMP推流上传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/RTMP推流上传.md)。

-   获取音视频数据

    您可以通过浏览器直接访问PlayURL地址的方式，即访问m3u8索引文件的方式来获取音视频数据。

    Android和iOS等移动平台，以及PC端的少数浏览器例如Microsoft Edge和Safari，支持通过访问m3u8文件的方式播放视频。Chrome等浏览器需要嵌入[Video.js](https://videojs.com/)等JavaScript脚本才能正常播放视频。


您在公共读写的Bucket中通过OSS上传和获取音视频，意味着任何人都有权限读写您的音视频数据，从而造成不必要的数据泄露和流量计费等问题。默认情况下，OSS Bucket的访问权限为私有，且拒绝任何来源的跨域请求。推荐您根据自身的使用场景，结合跨域资源共享CORS、防盗链、私有Bucket签名机制中的一种或多种方式，从而有效保护您的数据安全。

## 跨域资源共享CORS

如果不是通过浏览器直接访问OSS的音视频，而是先访问第三方的网页并在网页中嵌入了OSS的音视频，则很有可能遇到跨域问题，导致视频无法播放。这是因为当Web服务器和OSS不属于同一个域时，将违反同源策略，浏览器默认会拒绝该连接。

例如，Web服务器地址为http://118.xx.xx.xx:8080，浏览器访问该地址获取了网页，网页的JavaScript脚本里嵌入了视频，视频来源指向OSS Bucket。此时，浏览器需要再次向OSS发送请求，试图访问OSS中的视频数据。但是，浏览器识别到OSS Bucket的访问地址与http://118.xx.xx.xx:8080的网页地址不属于同一个域，则会先向OSS Bucket询问是否允许跨域请求。OSS Bucket默认不开启CORS配置，因此会拒绝浏览器的跨域请求，使得浏览器不能正常播放音视频。如下图所示：

![video](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5897298951/p147443.png)

您可以通过OSS的跨域资源共享，解决因跨域限制导致音视频无法正常播放的问题。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击Bucket列表，之后单击目标Bucket名称。

3.  单击**权限管理** \> **跨域设置**，在**跨域设置**区域单击**设置**。

4.  单击**创建规则**。

5.  在创建跨域规则页面配置各项参数。

    结合本文档示例场景，请将**来源**设置为http://118.xx.xx.xx:8080，其他参数设置请参见[设置跨域访问](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)。

    -   如果来源为具体的域名，请填写完整的域名，例如www.example.com，不可省略为example.com。
    -   如果来源为精准的IP地址，请填写包括协议类型和端口号在内的完整的IP地址，例如http://xx.xx.xx.xx:80，不可省略为xx.xx.xx.xx。
    浏览器对跨域配置通常会有数十秒至几分钟的缓存时间。如果希望跨域配置立即生效，建议清空浏览器缓存后刷新网页。


## 防盗链

CORS规则可以有效阻止其他网站服务器在网页中嵌入您的音视频资源。但如果您通过直接访问OSS Bucket的方式获取音视频，则CORS规则将失效。在这种情况下，您可以通过OSS防盗链对Bucket设置Referer白名单的机制，避免其他人盗取您的音视频资源。

默认情况下，Bucket允许空Referer，通过本地浏览器访问PlayURL的方式可以直接观看视频。为了防止音视频资源被其他人盗用，您可以将Bucket设置为不允许空Referer，并将您信任的域名或IP加入Referer白名单。此时，除Referer白名单以外的第三方访问音视频资源时均会被拒绝，并返回403 Forbidden错误。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击Bucket列表，之后单击目标Bucket名称。

3.  单击**权限管理** \> **防盗链**。

4.  在**防盗链**区域，单击**设置**。

    -   在**Referer**框中，填写域名或IP地址，例如\*.aliyun.com。

        您可以结合实际使用场景设置不同的Referer字段。Referer配置示例详情请参见[设置防盗链](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置防盗链.md)。

    -   在**空Referer**框中，选择不允许空Referer。

        **说明：**

        -   选择不允许空Referer且设置了Referer白名单，则只有HTTP或HTTPS header中包含Referer字段的请求才能访问OSS资源。
        -   选择不允许空Referer但未设置Referer白名单，效果等同于允许空Referer，即防盗链设置无效。
5.  单击**保存**。


## 私有Bucket签名机制

为了保护您的数据安全，OSS默认Bucket的读写权限为私有。因此当您需要向私有Bucket执行读取或写入操作时，需要使用签名机制向OSS声明您的操作权限。

当您向私有Bucket推流时，需要对推流地址进行签名后才能上传音视频文件。详情请参见[RTMP推流地址及签名](/intl.zh-CN/API 参考/关于LiveChannel的操作/RTMP推流地址及签名.md)。

使用Python SDK获取签名的推流地址示例如下：

```
import os
import oss2

access_key_id = "your_access_key_id"
access_key_secret = "your_access_key_secret"
bucket_name = "your_bucket_name"
endpoint = "your_endpoint"

# 创建Bucket实例。
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)

# 创建并配置流频道。
# 该索引文件包含3个ts文件，每个ts文件的时长为5秒。此示例中的5s时常为建议值，具体的时长取决于关键帧。
channel_name = "your_channel_name"
playlist_name = "your_playlist_name.m3u8"
frag_count_config = 3
frag_duration_config = 5
create_result = bucket.create_live_channel(
        channel_name,
        oss2.models.LiveChannelInfo(
            status = 'enabled',
            description = 'your description here',
            target = oss2.models.LiveChannelInfoTarget(
                playlist_name = playlist_name,
                frag_count = frag_count_config,
                frag_duration = frag_duration_config)))

# 获取RTMP推流签名地址。
# 示例中的expires是一个相对时间，表示从现在开始此次推流过期的秒数。
# 获取签名后的signed_url后即可使用推流工具直接进行推流。一旦连接上OSS，即使超出上面设置的expires也不会断流，OSS仅在每次推流连接时检查expires是否合法。
signed_rtmp_url = bucket.sign_rtmp_url(channel_name, playlist_name, expires=3600)
print(signed_rtmp_url)
```

OSS访问私有Bucket中的文件时，需要在URL中加上签名。HLS的访问机制为动态访问m3u8索引文件，根据索引文件的内容多次请求下载最新的ts文件，此过程中的每一次请求都需要在URL中加上签名。

![video2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5897298951/p147482.png)

为此，OSS对音视频数据的访问提供了动态签名机制，即只需在首次访问m3u8文件时在URL中添加`x-oss-process=hls/sign`, OSS将对返回的播放列表中的所有ts地址自动按照与m3u8完全相同的方式进行签名。

使用Python SDK动态签名机制访问音视频的示例如下：

```
# URL中拼接x-oss-process=hls/sign字段
DYNAMICY_SIGN_APPEND = "x-oss-process=hls/sign" 

# 获取观流的动态签名地址。
your_object_name = "test_rtmp_live/test.m3u8"
signed_download_url = bucket.sign_url('GET', your_object_name + "?" + DYNAMICY_SIGN_APPEND， expires=3600)
print(signed_download_url)
```

由于返回签名URL中可能包含的正斜线（/）、 问号（?）、and（&）等特殊字符，会被替换为'%2F'、'%3F'、'%26'等转义字符，导致返回的URL不符合规范。您需要对照如下格式对签名URL中的特殊字符进行更改。

`http://{your_bucket_name}.oss-cn-{your_region}.aliyuncs.com/{your_playlist_name.m3u8}?x-oss-process=hls/sign&OSSAccessKeyId=LTAI4G6FgJHrEMduCy****&Expires=1598168728&Signature=3rPOtC4yEwIWncGy7CDCgf****`

