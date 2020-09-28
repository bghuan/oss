# Create HLS streams based on OSS

OSS allows you to use Real-Time Messaging Protocol \(RTMP\) to ingest video and audio streams to OSS buckets and store the streams in the HTTP Live Streaming \(HLS\) format. OSS also provides various authentication and authorization methods to achieve fine-grained access control on audio and video data stored in buckets.

## Prerequisites

A bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/Create buckets.md).

## Basic operations

OSS allows you to use Real-Time Messaging Protocol \(RTMP\) to upload H.264-encoded video streams and Advanced Audio Coding \(AAC\)-encoded audio streams to OSS. You can obtain uploaded audio and video data by accessing the PlayURL of the LiveChannel.

-   Upload audio and video data
    1.  Call the [PutLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md) operation to create a LiveChannel.

        After the LiveChannel is created, PublishURL used to ingest streams through RTMP and PlayURL used to play audio and video data are returned .

    2.  Use PublishURL to ingest video and audio streams

        OSS stores uploaded audio and video data as HLS streams that include an m3u8 index file and multiple video files in the ts format. For more information, see [RTMP-based stream ingest](/intl.en-US/Developer Guide/Objects/Upload files/RTMP-based stream ingest.md).

-   Obtain audio and video data

    To obtain uploaded audio and video data, you can access PlayURL by using a browser to access the m3u8 index file.

    You can use Android and iOS mobile devices and a minority of browsers on PCs, such as Microsoft Edge and Safari, to access the m3u8 index file to play video files. If you use other browsers such as Chrome, you must embed JavaScript scripts such as [Video.js](https://videojs.com/) to the browser to play video files.


If you upload audio and video data to a public read/write bucket and access the data, all users can read your data in the bucket, which may lead to data leakage and incur additional traffic fees. By default, a bucket is private and denies all cross-origin requests. We recommend that you use one or more of the methods described in the following sections to protect data security based on scenarios.

## CORS

If you access an audio or video file embedded in a third-party website rather than directly access the audio and video file stored in OSS through a browser, the audio or video file may fail to be played because cross-origin requests are denied by the bucket that stores the audio or video file. The request is denied because the web server of the third-party website and the bucket that stores the audio or video file are in different origins, which does not meet the same-origin policy.

For example, the address of a web server is http://118.xx.xx.xx:8080. You access the address through a browser to access a website in which a video file stored in a OSS bucket is embedded. In this case, the browser sends a request to OSS to access the video file stored in the bucket. However, the browser identifies that the endpoint of the bucket and the address of the web server \(http://118.xx.xx.xx:8080\) are in different origins and send a request to confirm whether the bucket allows cross-origin requests. By default, CORS is disabled for OSS buckets and all cross-origin requests are denied. Therefore, the video file embedded in the website cannot be played. The following figure shows how the cross-origin request is denied.

![video](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1216521061/p147443.png)

You can enable CORS for the bucket that stores audio or video files to allow users to play the files on third-party websites.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click Buckets. On the Buckets page that appears, click the name of the bucket for which you want to enable CORS.

3.  Choose **Access Control** \> **Cross-Origin Resource Sharing \(CORS\)**. In the **Cross-Origin Resource Sharing \(CORS\)** section, click **Configure**.

4.  Click **Create Rule**.

5.  In the Create Rule dialog box that appears, configure parameters.

    In this example, set **Sources** to http://118.xx.xx.xx:8080. For the configurations of other parameters, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).

    -   If the source is a specific domain name, enter the complete domain name. For example, enter www.example.com but not example.com.
    -   If the source is a complete IP address, enter the complete IP address that includes the protocol type and port number. For example, enter http://xx.xx.xx.xx:80 but not xx.xx.xx.xx.
    A browser takes tens of seconds to a few minutes to cache the CORS configurations. To allow the CORS configurations to take effect immediately, we recommend that you clear your browser cache and then refresh the page.


## Hotlink protection

CORS can prevent your audio or video resources from being embedded in third-party websites. However, CORS configurations cannot prevent your audio or video resources stored in OSS buckets from being directly accessed. In this case, you can configure hotlink protection and specify a Referer whitelist for your buckets to prevent your audio or video resources from being accessed by unauthorized users.

By default, requests in which the Referer field is empty are allowed for OSS buckets. You can access the PlayURL of a video file stored in a bucket through a browser to view the video. To prevent your audio or video resources from being accessed by unauthorized users, you can configure hotlink protection for the bucket to deny requests in which the Referer field is empty and add trusted domain names or IP addresses to the Referer whitelist. In this case, requests from third-party domain names or IP addresses that are not included in the Referer whitelist are denied and 403 Forbidden is returned.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click Buckets. On the Buckets page that appears, click the name of the bucket for which you want to configure hotlink protection.

3.  Choose **Access Control** \> **Hotlink Protection**.

4.  In the **Hotlink Protection** section, click **Configure**.

    -   Enter domain names or IP addresses in the **Referer Whitelist** box. Example: \*.aliyun.com.

        You can configure different Referer fields based on scenarios. For more examples of Referer configurations, see [Configure hotlink protection](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure hotlink protection.md).

    -   Turn off **Allow Empty Referer** to deny requests in which the Referer field is empty.

        **Note:**

        -   If you turn off Allow Empty Referer and configure a Referer whitelist, only requests that includes Referer fields specified in the whitelist can access your resources in the bucket.
        -   If you turn off Allow Empty Referer and do not configure a Referer whitelist, the hotlink protection configuration does not take effect. All requests can access your resources whether the Referer field in the requests are empty.
5.  Click **Save**.


## Signature mechanism for private buckets

By default, the ACL of a bucket is private to protect data security. Therefore, when you send a request to a private bucket to read data from or write data to the bucket, you must add a signature in the request to declare your operation permissions on OSS.

When you ingest streams to a private bucket, you must sign the PublishURL to upload audio or video files to the bucket. For more information, see [RTMP ingest URLs and signatures](/intl.en-US/API Reference/LiveChannel-related operations/RTMP ingest URLs and signatures.md).

The following code provides an example on how to use OSS SDK for Python to obtain the PublishURL to ingest streams:

```
import os
import oss2

access_key_id = "your_access_key_id"
access_key_secret = "your_access_key_secret"
bucket_name = "your_bucket_name"
endpoint = "your_endpoint"

# Create a bucket.
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)

# Create and configure a LiveChannel.
# The index file includes three ts files whose duration is 5 seconds. The value of 5 seconds in this example is the recommended value. The duration depends on the key frame of the video file.
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

# Obtain the signed URL used to ingest streams through RTMP.
# The value of expires in the example indicates the duration in seconds after which the stream expires.
# After you obtain signed_url, you can use tools to ingest streams to OSS. OSS checks the value of expires only when a stream is connected to OSS. A stream that is connected to OSS does not expire even if the duration of the stream exceeds the value specified by expires.
signed_rtmp_url = bucket.sign_rtmp_url(channel_name, playlist_name, expires=3600)
print(signed_rtmp_url)
```

When you access an object in a private bucket by using the object URL, you must add a signature in the URL. When you access an HLS stream, a request is sent first to dynamically access the m3u8 index file of the stream and multiple requests are sent to download the latest ts files based on the content of the index file. You must add a signature to the URL in each request.

![video2](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1216521061/p147482.png)

To simplify the signature process, OSS provides a dynamic signature method. You can add the `x-oss-process=hls/sign` header to the URL in the request that is sent to access the m3u8 index file. OSS signs all URLs used to play the ts files in the returned playlist in the same way as that is specified by the header.

The following code provides an example on how to use OSS SDK for Python to dynamically add signatures to requests when you access audio or video files:

```
# Add the x-oss-process=hls/sign header to the URL in the request.
DYNAMICY_SIGN_APPEND = "x-oss-process=hls/sign" 

# Obtain the signed URL used to view the stream.
your_object_name = "test_rtmp_live/test.m3u8"
signed_download_url = bucket.sign_url('GET', your_object_name + "?" + DYNAMICY_SIGN_APPEND, expires=3600)
print(signed_download_url)
```

The slashes \(/\), question marks \(?\), and ampersands \(&\) in the returned signed URLs are replaced by escape characters such as '%2F', '%3F', and '%26'. Therefore, the returned signed URL may not conform to the specification. You must modify the escape characters in the signed URL based on the following format:

`http://{your_bucket_name}.oss-cn-{your_region}.aliyuncs.com/{your_playlist_name.m3u8}?x-oss-process=hls/sign&OSSAccessKeyId=LTAI4G6FgJHrEMduCy****&Expires=1598168728&Signature=3rPOtC4yEwIWncGy7CDCgf****`

