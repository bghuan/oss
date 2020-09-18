# Convert the storage class of an object

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), Archive and Cold Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md) and [Convert storage classes](/intl.en-US/Developer Guide/Storage classes/Convert storage classes.md) in the OSS Developer Guide.

The following code provides an example of how to convert the storage class of an object.

-   The following code provides an example of how to convert the storage class of an object from Standard or IA to Archive:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    import os
    
    # Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements. This example requires a bucket containing an object of the Standard or IA storage class.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    object_name = '<yourObjectName>'
    
    # Add a header that specifies the storage class. Set the storage class to Archive.
    headers = {'x-oss-storage-class': oss2.BUCKET_STORAGE_CLASS_ARCHIVE}
    
    # Modify the storage class of the object.
    bucket.copy_object(bucket.bucket_name, object_name, object_name, headers)
                        
    ```

-   The following code provides an example of how to convert the storage class of an object from Archive to IA:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    import os
    import time
    
    # Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations in OSS. We recommend that you log on with a RAM user account to call API operations or perform routine operations and maintenance. To create a RAM user account, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # This example uses China (Hangzhou) as the endpoint. Specify the actual endpoint based on your requirements. This example requires a bucket containing an object of the Archive storage class.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    object_name = '<yourObjectName>'
    
    # Obtain the object metadata.
    meta = bucket.head_object(object_name)
    
    # Check whether the storage class of the object is Archive. If the storage class of the source object is Archive, you must restore the object before you can modify the storage class. Wait for about one minute until the object is restored.
    if meta.resp.headers['x-oss-storage-class'] == oss2.BUCKET_STORAGE_CLASS_ARCHIVE:
        bucket.restore_object(object_name)
        while True:
            meta = bucket.head_object(object_name)
            if meta.resp.headers['x-oss-restore'] == 'ongoing-request="true"':
                time.sleep(5)
            else:
                break
    
    # Add a header that specifies the storage class. Set the storage class to IA.
    headers = {'x-oss-storage-class': oss2.BUCKET_STORAGE_CLASS_IA}
    
    # Modify the storage class of the object.
    bucket.copy_object(bucket.bucket_name, object_name, object_name, headers)
    ```


For more information about the storage classes and storage fees, see the [t4320.md\#section\_jmi\_2z4\_hka](/intl.en-US/Pricing/Billing items and methods/Overview.md) section in Billing items.

