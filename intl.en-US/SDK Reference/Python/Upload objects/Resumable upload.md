# Resumable upload

Before you upload an object to OSS by using resumable upload, you can specify a folder for the checkpoint file that stores resumable upload records. If a file fails to be uploaded because the network is abnormal or the program stops responding, the upload task is resumed from the position recorded in the checkpoint file to upload remaining bytes.

## Background information

You can use the oss2.resumable\_upload method to perform resumable upload for a specified object. The following table describes the parameters you can configure in the oss2.resumable\_upload method.

|Parameter|Description|Required|Default value|
|:--------|:----------|:-------|:------------|
|bucket|The name of the bucket.|Yes|None|
|key|The name of the object in OSS.|Yes|None|
|filename|The name of the local file to upload.|Yes|None|
|store|The directory where the checkpoint information is stored.|No|The .py-oss-upload directory created in the HOME directory.|
|headers|The HTTP header.|No|None|
|multipart\_threshold|Specifies a threshold for the object length. If the object length exceeds the threshold, multipart upload is used.|No|10 MB|
|part\_size|The size of the part.|No|Automatically calculated|
|progress\_callback|Specifies the upload progress callback function.|No|None|
|num\_threads|Specifies the number of concurrent upload threads.|No|1|

For more information about resumable upload, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md).

## Sample code

The following code provides an example on how to perform resumable upload by using multipart upload.

```
# -*- coding: utf-8 -*-
import oss2
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
# When the object length is greater than or equal to the value of the multipart_threshold optional parameter, multipart upload is used. The default value of multipart_threshold is 10 MB. If you do not use the store parameter to specify a directory, a directory named .py-oss-upload is created in the HOME directory to store the checkpoint information.
oss2.resumable_upload(bucket, '<yourObjectName>', '<yourLocalFile>')
```

OSS SDK for Python V2.1.0 and later supports the configuration of optional parameters in resumable upload. The following code provides an example on how to configure optional parameters in resumable upload:

```
# If you use the store parameter to specify a directory, the checkpoint information is stored in the specified directory. If you use the num_threads parameter to specify the number of concurrent upload threads, ensure that the value of oss2.defaults.connection_pool_size is greater than or equal to the number of concurrent upload threads. The default number of concurrent threads is 1.
oss2.resumable_upload(bucket, '<yourObjectName>', '<yourLocalFile>',
    store=oss2.ResumableStore(root='/tmp'),
    multipart_threshold=100*1024,
    part_size=100*1024,
    num_threads=4)
```

**Note:**

-   Multiple threads are used in resumable upload. Therefore, you do not need to encapsulate multiple upload threads when you call the resumable upload method. Otherwise, data may be transmitted repeatedly.
-   We recommend that you increase the part size when the network connection is stable. Otherwise, decrease the part size.

