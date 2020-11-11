# Manage symbolic links

This topic describes how to create a symbolic link and obtain the name of the object to which the symbolic link points.

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows. You can configure user-defined Object Meta for symbolic links.

**Note:** To obtain a symbolic link, you must have the read permissions on it. For more information about symbolic links, see [GetSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/GetSymlink.md).

The following code provides an example on how to create a symbolic link and obtain the name of the object to which the symbolic link points:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var targetObjectName = "<yourTargetObjectName>";
var symlinkObjectName = "<yourSymlinkObjectName>";
var objectContent = "More than just cloud." ;
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Upload the object to which the symbolic link points.
    byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
    MemoryStream requestContent = new MemoryStream(binaryData);
    client.PutObject(bucketName, targetObjectName, requestContent);
    // Create the symbolic link.
    client.CreateSymlink(bucketName, symlinkObjectName, targetObjectName);
    // Obtain the name of the object to which the symbolic link points.
    var ossSymlink = client.GetSymlink(bucketName, symlinkObjectName);
    Console.WriteLine("Target object is {0}", ossSymlink.Target);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

