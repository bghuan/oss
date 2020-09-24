# Configuration elements

This topic describes how to configure a lifecycle rule.

OSS allows you to call PutBucketLifecycle to configure lifecycle rules for a bucket. Lifecycle configurations of objects are in the XML format. Example:

```
<LifecycleConfiguration>
<Rule>
<ID>delete logs after 10 days</ID>
<Prefix>logs/</Prefix>
<Status>Enabled</Status>
<Expiration>
<Days>10</Days>
</Expiration>
</Rule>
<Rule>
<ID>delete doc</ID>
<Prefix>doc/</Prefix>
<Status>Disabled</Status>
<Expiration>
<CreatedBeforeDate>2017-12-31T00:00:00.000Z</CreatedBeforeDate>
</Expiration>
</Rule>
<Rule>
<ID>delete xx=1</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
        
```

Three lifecycle rules are defined in the preceding example:

-   The first rule is configured to delete objects whose names contain the logs/ prefix and that are not modified for more than 10 days.
-   The second rule is configured to delete objects whose names contain the doc/ prefix and that are not modified since December 31, 2017. However, this rule does not take effect because this rule is in the Disabled state.
-   The third rule is configured to convert the storage class of objects whose tagging configurations are xx=1 and that are not modified for more than 60 days to Archive.

The lifecycle configurations contain the following elements:

-   <ID\>: the ID that uniquely identifies each rule.
-   <Status\>: the status of the rule, which can be Enabled or Disabled. OSS runs only Enabled rules.
-   <Prefix\>: the prefix of object names.
-   <Expiration\>: the time or validity period to delete objects. The <CreatedBeforeDate\> child element specifies the absolute expiration time. The <Days\> child element specifies the relative expiration time.
    -   <CreatedBeforeDate\>: the date before which objects that are last modified expire and the operation to perform on these objects after they expire. All objects that are last modified before this date expire and the specified operation is performed on these objects.
    -   <Days\>: the number of days within which objects can be retained after they are last modified and the operation to perform on these objects after they expire. Objects that meet the specified conditions are retained for the specified period of time after the objects are last modified. The specified operation is performed on these objects when they expire.

## Versioning

The following section provides an example for configuration elements of a bucket in the unversioned, versioning-enabled, or versioning-suspended state:

-   Expiration: the expiration time or validity period to delete objects.

    If versioning is not enabled for the bucket, objects that match the conditions are permanently deleted. If versioning is enabled or suspended for the bucket, the object lifecycle rule is implemented:

    -   The Expiration element applies only to the current version of an object.
        -   If the current version is not a delete marker:
            -   When you configure the lifecycle rule on an object in a versioning-enabled bucket, OSS inserts a delete marker that has a unique version ID. The delete marker becomes the current version of the object. The previous current version is overwritten.
            -   When you configure the lifecycle rule on an object in a versioning-suspended bucket, OSS inserts a delete marker whose version ID is null. The delete marker becomes the current version of the object. The previous null version is deleted.
        -   If the current version is a delete marker:
            -   When one or more previous versions of the object remain and match the lifecycle rule, no actions are triggered.
            -   When only one version of the object remains and matches the lifecycle rule, the delete marker expires. OSS determines whether to remove the expired delete marker based on the ExpiredObjectDeleteMarker child element.

                **Note:**

                -   ExpiredObjectDeleteMarker: specifies whether to remove the expired delete marker. When you configure a lifecycle rule or initiate a delete operation without specifying the object version, the current version is overwritten. If you use lifecycle rules or delete operations to permanently delete all previous versions of an object, only the expired delete marker remains. If the ExpiredObjectDeleteMarker child element is set to true, OSS detects and then removes the expired delete marker to delete the object.
                -   You cannot configure ExpiredObjectDeleteMarker or tagging at the same time.
    -   NoncurrentVersionExpiration: the time or validity period to delete previous versions. The <NoncurrentDays\> child element specifies the relative expiration time within which previous versions can be retained. After the validity period, previous versions are permanently deleted.

        For example, a version is the current version of an object. On May 1, 2019, the PutObject operation was called, and this version became a previous version. In the NoncurrentVersionExpiration element, <NoncurrentDays\> is set to 3. The version is permanently deleted on May 4, 2019. Each time the PutObject operation is performed on an object, the current version of the object becomes the most recent previous version. The uploaded version becomes the current version of the object. OSS determines when a version becomes a previous version based on the creation time of the next version.

        **Note:** For more information about the configurations of other elements, see [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md).

-   <Transition\>: the conversion between storage classes.

    -   The <CreatedBeforeDate\> child element specifies the absolute conversion time. The <Days\> child element specifies the relative conversion time.
    -   The <StorageClass\> child element specifies the storage class to which the storage class of matched objects is converted.
    The <Transition\> element specifies the conversion between storage classes only for the current version of the object.

    **Note:** NoncurrentVersionTransition specifies the conversion between storage classes for previous versions of the object.

    -   The <NoncurrentDays\> child element specifies the relative conversion time within which previous versions can be retained. After the validity period, the storage class of previous versions is converted.
    -   The <StorageClass\> child element specifies the storage class to which the storage class of matched objects are converted.

