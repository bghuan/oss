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
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string SourceBucketName = "yourSourceBucketName";
    std::string CopyBucketName = "yourCopyBucketName";
    std::string SourceObjectName = "yourSourceObjectName";
    std::string CopyObjectName = "yourCopyObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    CopyObjectRequest request(CopyBucketName, CopyObjectName);
    request.setCopySource(SourceBucketName, SourceObjectName);
    request.setVersionId("yourSourceObjectVersionId");

    /* Copy the specified version of the source object.*/
    auto outcome = client.CopyObject(request);

    if (outcome.isSuccess()) {
        std::cout << "versionid:" << outcome.result().VersionId() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "CopyObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about how to copy small objects, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

## Copy a large object

To copy an object larger than 1 GB, you must split the object into parts and copy them sequentially by using UploadPartCopy.

By default, the UploadPartCopy operation uploads a part by copying data from the current version of an existing object. You can add versionId in the request header x-oss-copy-source as the condition to upload a part by copying data from a specified version of an existing object. Example: x-oss-copy-source : /SourceBucketName/SourceObjectName?versionId=111111.

**Note:** The value of SourceObjectName must be URL-encoded. The version ID of the copied object version is returned as the x-oss-copy-source-version-id header value in the response.

If versionId is not specified in the request and the current version of the source object is a delete marker, OSS returns 404 Not Found. If versionId is specified in the request and the specified version of the source object is a delete marker, OSS returns 400 Bad Request.

The following code provides an example on how to use multipart copy to copy large objects:

```
#include <alibabacloud/oss/OssClient.h>
#include <sstream>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string SourceBucketName = "yourSourceBucketName";
    std::string CopyBucketName = "yourCopyBucketName";
    std::string SourceObjectName = "yourSourceObjectName";
    std::string CopyObjectName = "yourCopyObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    auto getObjectMetaReq = GetObjectMetaRequest(SourceBucketName, SourceObjectName);
    getObjectMetaReq.setVersionId("yourSourceObjectVersionid");
    auto getObjectMetaResult = GetObjectMeta(getObjectMetaReq);
    if (! getObjectMetaResult.isSuccess()) {
        std::cout << "GetObjectMeta fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }
    /* Query the size of the source object.*/
    auto objectSize = getObjectMetaResult.result().ContentLength();

    /* Copy the large source object.*/
    InitiateMultipartUploadRequest initUploadRequest(BucketName, ObjectName, metaData);

    /* Initiate a multipart copy task.*/
    auto uploadIdResult = client.InitiateMultipartUpload(initUploadRequest);
    auto uploadId = uploadIdResult.result().UploadId();
    int64_t partSize = 100 * 1024;
    PartList partETagList;
    int partCount = static_cast<int>(objectSize / partSize);
    /* Calculate the number of parts to upload. */
    if (objectSize % partSize ! = 0) {
        partCount++;
    }

    /* Copy each part sequentially.*/
    for (int i = 1; i <= partCount; i++) {
        auto skipBytes = partSize * (i - 1);
        auto size = (partSize < objectSize - skipBytes) ? partSize : (objectSize - skipBytes);
        auto uploadPartCopyReq = UploadPartCopyRequest(CopyBucketName, CopyObjectName, SourceBucketName, SourceObjectName,uploadId, i + 1);
        uploadPartCopyReq.setCopySourceRange(skipBytes, skipBytes + size -1);
        uploadPartCopyReq.setVersionId("yourSourceObjectVersionid");
        auto uploadPartOutcome = client.UploadPartCopy(uploadPartCopyReq);
        if (uploadPartOutcome.isSuccess()) {
            Part part(i, uploadPartOutcome.result().ETag());
            partETagList.push_back(part);
        }
        else {
            std::cout << "UploadPartCopy fail" <<
            ",code:" << outcome.error().Code() <<
            ",message:" << outcome.error().Message() <<
            ",requestId:" << outcome.error().RequestId() << std::endl;
        }
    }

    /* Complete the multipart copy task. */
    CompleteMultipartUploadRequest request(CopyBucketName, CopyObjectName, partETagList, uploadId);
    auto outcome = client.CompleteMultipartUpload(request);

    if (outcome.isSuccess()) {
        std::cout << "versionid:" << outcome.result().VersionId() << std::endl;
    }
    else {
        /* Handle exceptions. */
        std::cout << "CompleteMultipartUpload fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about multipart copy, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).

