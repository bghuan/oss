FAQ 
========================

This topic describes problems you may encounter when you use the versioning feature provided by OSS and provides troubleshooting methods and solutions for the problems.

Storage cost 
---------------------------------

If you enable versioning for a bucket, you are charged for the storage of the current versions and previous versions of all objects in the bucket. The following example is used to describe the storage costs incurred in a month (30 days) for a versioning-enabled bucket. 

* On the first day of the month, you call PutObject to upload an object 4 GB in size to a Standard (LRS) bucket.

  

* On the 16th day of the month, you call PutObject to upload an object with the same name as the previously uploaded object but 5 GB in size to the same bucket.

  




After you upload the object on the 16th day, the original object you upload on the first day is retained as a previous version. The object you upload on the first day is stored in the bucket for 30 days in total. The object you uploaded on the 16th day is stored in the bucket for 15 days as the current version.

Therefore, the storage cost for the object in the month can be calculated based on the pay-as-you-go billing method by using the following formula: 4 GB x 0.02 USD/GB/Month + (5 GB x 0.02 USD/GB/Month)/2 = 0.13 USD.

For more information about the storage cost, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

Low response speed 
---------------------------------------

Question: Why does the response speed decrease significantly when the GetBucket (ListObjects) operation is called to list current object versions in a versioned bucket?

Cause: One or more objects in your bucket have a large number of previous versions or expired delete markers.

Troubleshooting:

* Call the GetBucketVersions (ListObjectVersions) operation to check whether the objects in your bucket have a large number of previous versions. For more information, see [GetBucketVersions(ListObjectVersions)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md).

  

* Use the bucket inventory feature to view the information about the objects in your bucket and check whether the objects have previous versions or expired delete markers. For more information, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).

  




Solution: Configure lifecycle rules for your bucket and specify the NonCurrentVersionExpiration and ExpiredObjectDeleteMarker operations in the rules to delete expired previous object versions and delete markers. For more information, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

