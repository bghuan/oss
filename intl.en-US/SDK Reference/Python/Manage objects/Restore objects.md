# Restore objects

This topic describes how to restore objects of the Archive and Cold Archive storage classes.

**Note:** You must restore an archived or cold archived object before you can read it. Do not call RestoreObject to perform restore operations on objects of which the storage class is not Archive and Cold Archive. For more information about the Archive and Cold Archive storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about related status codes, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) in API Reference.

## Restore archived objects

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore an archived object:

```
# -*- coding: utf-8 -*-
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com. 
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements. 
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')
objectName = '<yourObjectName>'

bucket.restore_object(objectName)            
```

## Restore cold archived objects

The status of a cold archived object throughout the restoration process is described as follows:

1.  By default, the cold archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore a cold archived object:

```
# -*- coding: utf-8 -*-

import oss2
from oss2.models import (RestoreJobParameters, 
                         RestoreConfiguration, 
                         RESTORE_TIER_EXPEDITED,  
                         RESTORE_TIER_STANDARD, 
                         RESTORE_TIER_BULK)

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to https://ram.console.aliyun.com. 
auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
# This example uses the endpoint of the China (Hangzhou) region. Specify the actual endpoint based on your requirements. 
bucket = oss2.Bucket(auth, 'http://oss-cn-hangzhou.aliyuncs.com', '<yourBucketName>')

object_name = "<yourColdArchiveObjectName>"

# Refer to the following code if you set the storage class of the object to upload to Cold Archive.
# bucket.put_object(object_name, '<yourContent>', headers={"x-oss-storage-class": oss2.BUCKET_STORAGE_CLASS_COLD_ARCHIVE})

# Configure the restore mode of the cold archived object. # RESTORE_TIER_EXPEDITED: The object is restored within one hour.
# RESTORE_TIER_STANDARD: The object is restored within two to five hours.
# RESTORE_TIER_BULK: The object is restored within five to twelve hours.
job_parameters = ResotreJobParameters(RESTORE_TIER_STANDARD)

# Configure parameters. For example, set the restore mode of the object to Standard and set the duration for which the object can remain in the restored state to two days. 
# The days parameter indicates the duration for which the object can remain in the restored state. The default value is one day. This parameter applies to archived objects and cold archived objects. 
# The job_parameters parameter indicates the restore mode of the object. This parameter applies only to cold archived objects.
restore_config= RestoreConfiguration(days=2, job_parameters=job_parameters)

# Initiate a restore request.
bucket.restore_object(object_name, input=restore_config)
```

