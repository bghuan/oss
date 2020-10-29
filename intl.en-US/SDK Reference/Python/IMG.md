# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

For the complete IMG code, visit [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/image.py).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    
    # This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
    endpoint = 'https://oss-cn-hangzhou.aliyuncs.com'
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    access_key_id = '<yourAccessKeyId>'
    access_key_secret = '<yourAccessKeySecret>'
    # The name of the bucket.
    bucket_name = '<yourBucketName>'
    # The name of the image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
    key = '<yourObjectName>'
    # Specify the name of the processed image.
    new_pic = '<LocalFileName>'
    
    # Specify the bucket instance. You must use the bucket instance to call all object-related methods.
    bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    
    # If the image does not exist in the specified bucket, the image must be uploaded to the requested bucket.
    # bucket.put_object_from_file(key, '<yourLocalFile>')
    
    # After you resize the image to a height and width of 100 pixels, save the image to your local computer.
    style = 'image/resize,m_fixed,w_100,h_100'
    bucket.get_object_to_file(key, new_pic, process=style)
    
    # After the IMG is complete, you can delete the source image if you no longer use the image in the bucket.
    # bucket.delete_object(key)
    # If you no longer use the processed image, you can delete the processed image.
    # os.remove(new_pic)
                                
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    
    # This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
    endpoint = 'https://oss-cn-hangzhou.aliyuncs.com'
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    access_key_id = '<yourAccessKeyId>'
    access_key_secret = '<yourAccessKeySecret>'
    # The name of the bucket.
    bucket_name = '<yourBucketName>'
    # The name of the image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
    key = '<yourObjectName>'
    # Specify the name of the processed image.
    new_pic = '<LocalFileName>'
    
    # Specify the bucket instance. You must use the bucket instance to call all object-related methods.
    bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    
    # If the image does not exist in the specified bucket, the image must be uploaded to the requested bucket.
    # bucket.put_object_from_file(key, '<yourLocalFile>')
    
    # After you resize the image to a height and width of 100 pixels, rotate the image 90°, and save the image to your local computer.
    style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
    bucket.get_object_to_file(key, new_pic, process=style)
    # After the IMG is complete, you can delete the source image if you no longer use the image in the bucket.
    # bucket.delete_object(key)
    # If you no longer use the processed image, you can delete the processed image.
    # os.remove(new_pic)
                                
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use the style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
# -*- coding: utf-8 -*-
import os
import oss2

# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
endpoint = 'https://oss-cn-hangzhou.aliyuncs.com'
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
# The name of the bucket.
bucket_name = '<yourBucketName>'
# The name of the image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
key = '<yourObjectName>'
# Specify the name of the processed image.
new_pic = '<LocalFileName>'

# Specify the bucket instance. You must use the bucket instance to call all object-related methods.
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
# If the image does not exist in the specified bucket, the image must be uploaded to the requested bucket.
# bucket.put_object_from_file(key, '<yourLocalFile>')

# Specify the image style.
style = 'style/oss-pic-style-w-100'
# Save the processed image to your local computer.
bucket.get_object_to_file(key, new_pic, process=style)
# After the IMG is complete, you can delete the source image if you no longer use the image in the bucket.
# bucket.delete_object(key)
# If you no longer use the processed image, you can delete the processed image.
# os.remove(new_pic)
```

## IMG persistence

You can call the ImgSaveAs operation to save the images to the bucket where the source images are stored. The following code provides an example on how to save an processed image:

```
# -*- coding: utf-8 -*-
import os
import base64
import oss2

# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
endpoint = 'https://oss-cn-hangzhou.aliyuncs.com'
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
# The name of the bucket where the source image is stored.
source_bucket_name = '<yourBucketName>'
# Specify the bucket that stores the processed image. The bucket must be within the same region as the source bucket.
taget_bucket_name = '<yourTargetBucketName>'
# The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
source_image_name = '<sourceObjectName>'

# Specify the bucket instance. You must use the bucket instance to call all object-related methods.
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, source_bucket_name)

# Resize the image to a height and width of 100 pixels.
style = 'image/resize,m_fixed,w_100,h_100'
# Specify the name of the processed image. If the access path is included, the image is stored in the specified path. Example: example/example-resize.jpg.
target_image_name = '<targetObjectName>'
process = "{0}|sys/saveas,o_{1},b_{2}".format(style, 
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(target_image_name))),
    oss2.compat.to_string(base64.urlsafe_b64encode(oss2.compat.to_bytes(taget_bucket_name))))
result = bucket.process_object(source_image_name, process)
print(result)
```

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
# -*- coding: utf-8 -*-
import oss2

# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements.
endpoint = 'https://oss-cn-hangzhou.aliyuncs.com'
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
access_key_id = '<yourAccessKeyId>'
access_key_secret = '<yourAccessKeySecret>'
# The name of the bucket.
bucket_name = '<yourBucketName>'
# The name of the image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
key = '<yourObjectName>'

# Specify the bucket instance. You must use the bucket instance to call all object-related methods.
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
# If the image does not exist in the specified bucket, the image must be uploaded to the requested bucket.
# bucket.put_object_from_file(key, '<yourLocalFile>')
# After you resize the image to a height and width of 100 pixels, rotate the image 90°.
style = 'image/resize,m_fixed,w_100,h_100/rotate,90'
# Generate a signed object URL that includes IMG parameters. Set the validity period to 10 minutes. Unit: seconds.
url = bucket.sign_url('GET', key, 10 * 60, params={'x-oss-process': style})
print(url)
                    
```

