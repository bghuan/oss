# List objects

This topic describes how to list all objects, objects whose names contain a specified prefix, and objects and subfolders within a specified folder of a bucket.

## Background information

You can call the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list up to 1,000 objects in a bucket. You can configure the parameters to use different methods to list objects. For example, you can configure the parameters to list objects from a specified start position, list objects and subfolders in specified folders, and list objects by page. The GetBucket \(ListObjects\) and GetBucketV2 \(ListObjectsV2\) operations are different in the following aspects:

-   By default, when you use the GetBucket \(ListObjects\) operation to list objects, the bucket owner information is included in the response.
-   When you use the GetBucketV2 \(ListObjectsV2\) operation to list objects, you can set the fetch\_owner parameter to specify whether to include the information about the bucket owner in the response.

    **Note:** To list objects in a versioned bucket, we recommend that you use the GetBucketV2 \(ListObjectsV2\) operation.


The following tables describe the parameters that you must configure when you use the GetBucket \(ListObjects\) or GetBucketV2 \(ListObjectsV2\) operation to list objects.

-   Use GetBucket \(ListObjects\) to list objects

    The following table describes the parameter you can configure when you use GetBucket \(ListObjects\) to list objects.

    |Parameter|Description|
    |:--------|:----------|
    |prefix|The prefix that the names of returned objects must contain.|
    |delimiter|The character used to group objects by name.|
    |marker|The position from which the list operation starts. All objects whose names are lexicographically greater than the value of this parameter are returned.|

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    The following table describes the parameters you can configure when you use GetBucketV2 \(ListObjectsV2\) to list objects.

    |Parameter|Description|
    |:--------|:----------|
    |prefix|The prefix that the names of returned objects must contain.|
    |delimiter|The character used to group objects by name.|
    |startAfter|The position from which the list operation starts. All objects are returned in lexicographic order starting from this position.|
    |fetchOwner|Specifies whether to include the owner information about the bucket in the response.     -   true: The response includes the owner information.
    -   false: The response does not include the owner information. |


## Simple list

-   Use GetBucket \(ListObjects\) to list objects

    The following code provides an example on how to use GetBucket \(ListObjects\) to list 10 objects in a specified bucket:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    from itertools import islice
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    for b in islice(oss2.ObjectIterator(bucket), 10):
        print(b.key)
                        
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list 10 objects in a specified bucket:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    from itertools import islice
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    for obj in islice(oss2.ObjectIteratorV2(bucket), 10):
        print(obj.key)
                        
    ```


## List all objects in a bucket

-   Use GetBucket \(ListObjects\) to list all objects in a bucket

    The following code provides an example on how to use GetBucket \(ListObjects\) to list all objects in a specified bucket:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects in the specified bucket.
    for obj in oss2.ObjectIterator(bucket):
        print(obj.key)
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list all objects in a bucket

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list all objects in a specified bucket:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects in the specified bucket.
    for obj in oss2.ObjectIteratorV2(bucket):
        print(obj.key)
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects in a bucket and include the information about the bucket owner in the response

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list all objects in a specified bucket and how to specify the fetch\_owner parameter to include the bucket owner information in the response:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects in the specified bucket and specify the fetch_owner parameter to include the information about the bucket owner in the response.
    for obj in oss2.ObjectIteratorV2(bucket, fetch_owner=True):
        print(obj.key)
        print('file owner display name: ' + obj.owner.display_name)
        print('file owner id: ' + obj.owner.id)
    ```


## List objects whose names contain a specified prefix

In the following examples, four objects are stored in the bucket: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. A forward slash \(/\) is used as the folder delimiter.

-   Use GetBucket \(ListObjects\) to list objects whose names contain a specified prefix

    The following code provides an example on how to use GetBucket \(ListObjects\) to list objects whose names contain a specified prefix:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects in the fun folder and its subfolders.
    for obj in oss2.ObjectIterator(bucket, prefix='fun/'):
        print(obj.key)
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects whose names contain a specified prefix:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects in the fun folder and its subfolders.
    for obj in oss2.ObjectIteratorV2(bucket, prefix='fun/'):
        print(obj.key)
    ```

-   Response

    ```
    fun/movie/001.avi
    fun/movie/007.avi
    fun/test.jpg
    ```


## List all objects from a specified start position

-   Use GetBucket \(ListObjects\) to list all objects from a specified start position

    You can configure the marker parameter to specify the start position from which the list operation starts. All objects are returned in lexicographic order starting from this position. In the following example, the bucket stores four objects: x1.txt, x2.txt, z1.txt, and z2.txt.

    The following code provides an example on how to use GetBucket \(ListObjects\) to list all objects from a specified start position:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects whose names are lexicographically greater than the value of marker. The object whose name is equal to the value of marker is not returned.
    for obj in oss2.ObjectIterator(bucket, marker="x2.txt"):
        print(obj.key)
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list all objects from a specified start position

    You can configure the start\_after parameter to specify the start position from which the list operation starts. All objects are returned in lexicographic order starting from this position. In the following example, the bucket stores four objects: x1.txt, x2.txt, z1.txt, and z2.txt.

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list all objects from a specified start position:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List all objects whose names are lexicographically greater than the value of start_after. The object whose name is equal to the value of start_after is not returned.
    for obj in oss2.ObjectIteratorV2(bucket, start_after="x2.txt"):
        print(obj.key)
    ```


## List objects and subfolders in a specified folder

OSS does not use folders. All elements are stored as objects. To simulate a folder on the OSS console, you actually create a 0 MB object whose name ends with a forward slash \(/\). This object can be uploaded and downloaded. By default, the OSS console displays an object whose name ends with a forward slash \(/\) as a folder.

The delimiter and prefix parameters can be used to simulate folder functions.

-   If you set prefix to the name of a folder, objects and subfolders whose names contain the prefix are listed.
-   If you set delimiter to a forward slash \(/\), only the objects and subfolders in the folder are listed. The objects and folders in subfolders are not listed.

The following examples show how to use GetBucket \(ListObjects\) and GetBucketV2 \(ListObjectsV2\) to list objects and subfolders in a specified folder.

-   Use GetBucket \(ListObjects\) to list objects and subfolders in a specified folder

    The following code provides an example on how to use GetBucket \(ListObjects\) to list objects and subfolders in a specified folder:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List objects and subfolders in the fun folder. Objects in the subfolders are not listed.
    for obj in oss2.ObjectIterator(bucket, prefix = 'fun/', delimiter = '/'):
        # Use is_prefix to determine whether obj is a folder.
        if obj.is_prefix():  # Specify the operation to perform if obj is determined as a folder.
            print('directory: ' + obj.key)
        else:                # Specify the operation to perform if obj is determined as an object.
            print('file: ' + obj.key)
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list objects and subfolders in a specified folder

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list objects and subfolders in a specified folder:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    # List objects and subfolders in the fun folder. Objects in the subfolders are not listed. If you do not need the bucket owner information, you can leave the fetch_owner parameter unspecified.
    for obj in oss2.ObjectIteratorV2(bucket, prefix = 'fun/', delimiter = '/', fetch_owner=True):
        # Use is_prefix to determine whether obj is a folder.
        if obj.is_prefix():  # Specify the operation to perform if obj is determined as a folder.
            print('directory: ' + obj.key)
        else:                # Specify the operation to perform if obj is determined as an object.
            print('file: ' + obj.key)
            print('file owner display name: ' + obj.owner.display_name)
            print('file owner id: ' + obj.owner.id)
    ```

-   Response

    ```
    directory: fun/movie/
    file: fun/test.jpg
    ```


## List the sizes of objects within a specified folder

-   Use GetBucket \(ListObjects\) to list the sizes of objects within a specified folder

    The following code provides an example on how to use GetBucket \(ListObjects\) to list the sizes of objects within a specified folder:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    def CalculateFolderLength(bucket, folder):
        length = 0
        for obj in oss2.ObjectIterator(bucket, prefix=folder):
            length += obj.size
        return length
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    for obj in oss2.ObjectIterator(bucket, delimiter='/'):
        if obj.is_prefix():  # Specify the operation to perform if obj is determined as a folder.
            length = CalculateFolderLength(bucket, obj.key)
            print('directory: ' + obj.key + '  length:' + str(length) + "Byte.")
        else: # Specify the operation to perform if obj is determined as an object.
            print('file:' + obj.key + '  length:' + str(obj.size) + "Byte.")
    ```

-   Use GetBucketV2 \(ListObjectsV2\) to list the sizes of objects within a specified folder

    The following code provides an example on how to use GetBucketV2 \(ListObjectsV2\) to list the sizes of objects within a specified folder:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    def CalculateFolderLength(bucket, folder):
        length = 0
        for obj in oss2.ObjectIteratorV2(bucket, prefix=folder):
            length += obj.size
        return length
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
    
    for obj in oss2.ObjectIteratorV2(bucket, delimiter='/'):
        if obj.is_prefix():  # Specify the operation to perform if obj is determined as a folder.
            length = CalculateFolderLength(bucket, obj.key)
            print('directory: ' + obj.key + '  length:' + str(length) + "Byte.")
        else: # Specify the operation to perform if obj is determined as an object.
            print('file:' + obj.key + '  length:' + str(obj.size) + "Byte.")
    ```


