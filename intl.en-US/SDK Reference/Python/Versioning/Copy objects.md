# Copy objects

This topic describes how to copy objects in a versioned bucket. You can use CopyObject to copy an object that is up to 1 GB in size and use UploadPartCopy to copy an object larger than 1 GB.

## Copy a small object

You can use CopyObject to copy an object that is up to 1 GB in size from a source bucket to a destination bucket within the same region.

-   By default, the x-oss-copy-source header in a CopyObject request specifies the current version of the object to copy. If the current version of the object to copy is a delete marker, 404 Not Found is returned to indicate that the object does not exist. You can add a version ID in the x-oss-copy-source header to copy a specified object version. Delete markers cannot be copied.
-   You can copy a previous version of an object to the same bucket. The copied previous version becomes the current version of the object. This way, you can restore an object to a specified previous version.
-   If versioning is enabled for the destination bucket, OSS generates a unique version ID for the destination object. The version ID is returned in the response as the x-oss-version-id header value. If versioning is disabled or suspended for the destination bucket, OSS generates a version whose version ID is null for the destination object and overwrites the original version whose version ID is null.
-   Append objects in a bucket with versioning enabled or suspended cannot be copied.

The following code provides an example on how to copy a small object:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

# Copy the specified version of the object.
params = dict()
params['versionId'] = '<yourObjectVersionId>'
result = bucket.copy_object('<yourBucketName>',  '<yourSourceObjectName>', '<yourDestinationObjectName>', params=params)
# Display the version ID of the destination object.
print('result.versionid:', result.versionid)
```

For more information about how to copy small objects, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

## Copy a large object

To copy an object larger than 1 GB, you must split the object into parts and copy them sequentially by using UploadPartCopy.

By default, the UploadPartCopy operation uploads a part by copying data from the current version of an existing object. You can add versionId in the request header x-oss-copy-source as the condition to upload a part by copying data from a specified version of an existing object. Example: x-oss-copy-source : /SourceBucketName/SourceObjectName?versionId=111111.

**Note:** The value of SourceObjectName must be URL-encoded. The version ID of the copied object version is returned as the x-oss-copy-source-version-id header value in the response.

If versionId is not specified in the request and the current version of the source object is a delete marker, OSS returns 404 Not Found. If versionId is specified in the request and the specified version of the source object is a delete marker, OSS returns 400 Bad Request.

The following code provides an example on how to use multipart copy to copy large objects:

```
# -*- coding: utf-8 -*-
import os
import oss2
from oss2 import determine_part_size
from oss2.models import PartInfo

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

src_object_name = '<yourSrcObject>'
dest_object_name = '<yourDestObject>'

params = dict()
params['versionId'] = '<yourObjectVersionId>'

# Query the size of the specified version of the source object.
head_info = bucket.head_object(src_object_name, params=params)
total_size = head_info.content_length
print('src object size:', total_size)

# Use the determine_part_size method to determine the part size.
part_size = determine_part_size(total_size, preferred_size=100 * 1024)
print('part_size:', part_size)

# Initiate a multipart upload task.
upload_id = bucket.init_multipart_upload(dest_object_name).upload_id
parts = []

# Upload the parts one by one.
part_number = 1
offset = 0
while offset < total_size:
    num_to_upload = min(part_size, total_size - offset)
    end = offset + num_to_upload - 1;
    result = bucket.upload_part_copy(bucket.bucket_name, src_object_name, (offset, end), dest_object_name, upload_id, part_number, params=params)
    # Save the part information.
    parts.append(PartInfo(part_number, result.etag))

    offset += num_to_upload
    part_number += 1

# Complete the multipart upload task.
result = bucket.complete_multipart_upload(dest_object_name, upload_id, parts)
# Display the version ID of the destination object.
print('result version id:', result.versionid)
# Obtain the metadata of the destination object.
head_info = bucket.head_object(dest_object_name)
# Display the size of the destination object.
dest_object_size = head_info.content_length
print('dest object size:', dest_object_size)
# Compare the size of the destination object with that of the source object.
assert dest_object_size == total_size
```

For more information about multipart copy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).

