# Determine whether an object exists

This topic describes how to determine whether an object exists.

The following code provides an example on how to determine whether an object exists:

```
const OSS = require('ali-oss');

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});

async function isExistObject(name, options = {}) {
  try {
      await store.head(name, options);
      console.log('The object exists.')
   }  catch (error) {
      if (error.code === 'NoSuchKey') {
        console.log('The object does not exist.')
      }
   }
}
// Determine whether an object exists in an unversioned bucket.
// Specify the complete name of the object excluding the bucket name. Example: example/test.txt.
const name = '<Your ObjectName>'
isExistObject(name)

// Determine whether an object with a specified version ID exists in a versioned bucket.
const options = {
    // Specify the version ID of the object.
    versionId: '<Your Object VersionId>' 
}
isExistObject(name, options)
```

