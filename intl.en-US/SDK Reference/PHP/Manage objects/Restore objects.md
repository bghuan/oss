# Restore objects

You must restore an Archive or a Cold Archive object before you can read it. This topic describes how to restore Archive and Cold Archive objects.

**Note:** Do not call RestoreObject to restore an object whose storage class is not Archive or Cold Archive.

For more information about the Archive and Cold Archive storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). For more information about how to restore an Archive or a Cold Archive object, see [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md).

## Restore Archive objects

The status of an archived object throughout the restoration process is described as follows:

1.  By default, the archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. It takes about one minute to complete the restore task.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. By default, the restored state lasts 24 hours and can be extended by another 24 hours after you call RestoreObject again. During one restoration process, RestoreObject can be called on an object for up to seven times. In other words, an object can remain in the restored state for up to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore an Archive object:

```
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object = "<yourObjectName>";

try {
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);

    $ossClient->restoreObject($bucket, $object);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}
print(__FUNCTION__ . ": OK" . "\n");
        
```

## Restore Cold Archive objects

The following section describes the status of a Cold Archive object throughout the restoration process:

1.  By default, the cold archived object is in the frozen state.
2.  After you submit a restore request, the object is in the restoring state. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object as follows:
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours. If the JobParameters element is not passed in, the default restore mode is Standard.
    -   Bulk: The object is restored within five to twelve hours.
3.  After the server completes the restore task, the object enters the restored state and you can read the object. You can specify the duration for which the object can remain in the restored state, which ranges from one to seven days.
4.  After the restored state expires, the object returns to the frozen state.

The following code provides an example on how to restore a Cold Archive object:

```
<? php
if (is_file(__DIR__ . '/../autoload.php')) {
    require_once __DIR__ . '/../autoload.php';
}
if (is_file(__DIR__ . '/../vendor/autoload.php')) {
    require_once __DIR__ . '/../vendor/autoload.php';
}

use OSS\OssClient;
use OSS\Core\OssException;
use OSS\Model\RestoreConfig;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
$object= "<yourObjectName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

// Set the restoration priority to Standard and the duration for which the object can remain in the restored state to 5 days.
$config = new RestoreConfig(5, "Standard");
$options = array(
    OssClient::OSS_RESTORE_CONFIG => $config
);

try {
    // Restore the object.
    $ossClient->restoreObject($bucket, $object, $options);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
            
```

