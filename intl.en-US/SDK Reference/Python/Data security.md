# Data security

OSS SDK for Java uses MD5 verification and CRC-64 to ensure data integrity when you upload, download, and copy objects.

## MD5 verification

If you configure Content-MD5 in an object upload request, OSS calculates the MD5 hash of the uploaded object. If the calculated MD5 hash is different from the MD5 hash configured in the upload request, InvalidDigest is returned. This allows OSS to ensure data integrity for object upload. If InvalidDigest is returned, you must upload the object again.

The following code provides an example on how to perform MD5 verification in object upload:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
object_name = '<yourObjectName>'
content = '<yourContent>'

# Calculate the MD5 hash of the uploaded content.
content_md5 = oss2.utils.content_md5(content);
print('content_md5', content_md5)

# If the upload request contains the Content-MD5 header, the OSS server verifies the MD5 hash of the uploaded content.
headers = dict()
headers['Content-MD5'] = content_md5
bucket.put_object(object_name, content, headers=headers)
```

**Note:** MD5 verification can be used for put\_object, append\_object, post\_object, and upload\_part.

## CRC-64

**Note:**

-   CRC-64 can be used for put\_object, get\_object, append\_object, and upload\_part. By default, CRC-64 is enabled when you upload an object. If the CRC-64 value calculated on the client is different from the CRC-64 value returned by the OSS server, InconsistentError is returned.
-   CRC-64 is not supported in range download.
-   CRC-64 consumes CPU resources and slows down uploads and downloads.

-   CRC-64 in object download

    The following code provides an example on how to perform CRC-64 in object download:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    object_name = '<yourObjectName>'
    
    # Check whether CRC-64 is enabled by default.
    print('bucket.enable-crc:',  bucket.enable_crc)
    
    # The return value of bucket.get_object is a file-like and iterable object.
    object_stream = bucket.get_object(object_name)
    print(object_stream.read())
    
    # You must call read() to read the object from the stream returned by get_object before you can calculate the CRC-64 value of the object.
    if object_stream.client_crc ! = object_stream.server_crc:
        print "The CRC checksum between client and server is inconsistent!"
    ```

-   CRC-64 in append upload

    If you set init\_crc in append upload, CRC-64 is enabled by default.

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    object_name = "<yourAppendObjectName>"
    first_content = "<yourFirstContent>"
    second_content = "<yourSecondContent>"
    
    # Set init_crc in the first append.
    # When init_crc is set, OSS SDK for Python performs CRC-64 for the returned result.
    result = bucket.append_object(object_name, 0, first_content, init_crc=0)
    
    # Set init_crc in the second append.
    # Set init_crc to the CRC-64 value of the uploaded object.
    result = bucket.append_object(object_name, result.next_position, second_content, init_crc=result.crc)
    ```


