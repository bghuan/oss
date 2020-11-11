# Delete buckets

If you no longer use a bucket, delete it to avoid incurring unnecessary fees.

-   All objects in the bucket have been deleted. For more information about how to delete objects, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md). If you have a large number of objects, we recommend that you use lifecycle rules. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   Parts generated from incomplete multipart upload tasks or resumable upload tasks have been deleted. For more information about how to delete parts in a bucket, see [Manage parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md).
-   All LiveChannels in the bucket have been deleted. For more information about LiveChannels, see [RTMP-based stream ingest](/intl.en-US/Developer Guide/Objects/Upload files/RTMP-based stream ingest.md).

**Warning:** You cannot recover deleted buckets. Exercise caution when you perform this operation.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Basic Settings** \> **Delete Bucket**.

4.  Click **Delete Bucket**. In the message that appears, click **OK**.


