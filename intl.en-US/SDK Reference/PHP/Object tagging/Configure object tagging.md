# Configure object tagging

OSS allows you to configure object tagging to classify objects. You can configure object lifecycle rules and ACLs based on tags.

## Usage notes

The object tagging feature uses a key-value pair to tag an object. When you configure object tagging, take note of the following items:

-   Only the bucket owner and authorized users have read and write permissions on object tags. These permissions are independent of object ACLs.
-   You can add tags to an object when you upload the object. You can also add tags to an uploaded object or modify the tags of an object.

    For more information about configure object tagging, see [PutObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/PutObjectTagging.md).

-   You can add up to 10 tags to an object. The tags added to an object must have unique tag keys.
-   A tag key can be up to 128 bytes in length. A tag value can be up to 256 bytes in length.
-   Tag keys and values are case-sensitive.
-   The key and value of a tag can contain letters, digits, spaces, and the following special characters:

    + - =. \_:/

-   The Last-Modified value of an object is not updated when its tags are changed.

For more information about object tags, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Add tags to an object when you upload the object

The following examples describe how to add tags to an object when you upload the object by using simple upload, multipart upload, and append upload.

-   Add tags to an object when you upload the object by using simple upload

    The following code provides an example on how to add tags to an object when you upload the object by using simple upload:

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
    $endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    $bucket= "<yourBucketName>";
    // yourObjectName indicates the complete name of the object to upload, including the object extension. Example: exmple/test.jpg
    $object= "<yourObjectName>";
    $content= "hello world";
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
    
    // Configure the tags you want to add to the object.
    $options = array(
          OssClient::OSS_HEADERS => array(
                  'x-oss-tagging' => 'key1=value1&key2=value2&key3=value3',
    ));
      
    try {
          // Upload the object by using simple upload.
          $ossClient->putObject($bucket, $object, $content, $options);
    } catch (OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");  
    ```

-   Add tags to an object when you upload it by using multipart upload

    The following code provides an example on how to add tags to an object when you upload it by using multipart upload:

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
    $endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    $bucket= "<yourBucketName>";
    $object= "<yourObjectName>";
    $file = "<yourFilePath>";
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
    
    // Configure the tags you want to add to the object.
    $options = array(
        OssClient::OSS_CHECK_MD5 => true,
        OssClient::OSS_PART_SIZE => 1,
        OssClient::OSS_HEADERS => array(
              'x-oss-tagging' => 'key1=value1&key2=value2&key3=value3',
        ),
    );
    
    try {
        // Upload the object by using multipart upload.
        $ossClient->multiuploadFile($bucket, $object, $file, $options);
    } catch (OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");
    ```

-   Add tags to an object when you upload it by using append upload

    The following code provides an example on how to add tags to an object when you upload it by using append upload:

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
    $endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
    $bucket= "<yourBucketName>";
    $object= "<yourObjectName>";
    $content_array = array('Hello OSS', 'Hi OSS');
    
    $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);
    
    // Configure the tags you want to add to the object.
    $options = array(
          OssClient::OSS_HEADERS => array(
                  'x-oss-tagging' => 'key1=value1&key2=value2',
    ));
      
    try {
        // Upload the object by using append upload.
        $position = $ossClient->appendObject($bucket, $object, $content_array[0], 0, $options);
        $position = $ossClient->appendObject($bucket, $object, $content_array[1], $position);
    } catch (OssException $e) {
        printf(__FUNCTION__ . ": FAILED\n");
        printf($e->getMessage() . "\n");
        return;
    }
    
    print(__FUNCTION__ . ": OK" . "\n");
    ```


## Add tags to an uploaded object or modify the tags added to an object

The following code provides an example on how to add tags to an uploaded object or modify the tags added to an object:

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
use OSS\Model\TaggingConfig;
use OSS\Model\Tag;

// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
$accessKeyId = "<yourAccessKeyId>";
$accessKeySecret = "<yourAccessKeySecret>";
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
$endpoint = "https://oss-cn-hangzhou.aliyuncs.com";
$bucket= "<yourBucketName>";
// yourObjectName indicates the complete name of the uploaded object, including the object extension. Example: example/test.jpg.
$object= "<yourObjectName>";

$ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint, false);

// Configure the tags you want to add to the object.
$config = new TaggingConfig();
$config->addTag(new Tag("key1", "value1"));
$config->addTag(new Tag("key2", "value2"));

try {
    $ossClient->putObjectTagging($bucket, $object, $config);
} catch (OssException $e) {
    printf(__FUNCTION__ . ": FAILED\n");
    printf($e->getMessage() . "\n");
    return;
}

print(__FUNCTION__ . ": OK" . "\n");
```

