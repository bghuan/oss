# IMG

Image Processing \(IMG\) provided by OSS is a secure, cost-effective, and highly reliable image processing service that processes large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your Region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
    });
    // After you resize the image to a height and width of 100 pixels, save the image to your local computer.
    async function scale () {
      try {
        let result = await client.get('<OSSObjectName>', '<LocalFileName>', { process: 'image/resize,m_fixed,w_100,h_100'});
      } catch (e) {
        console.log(e);
      }
    }
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<Your Region>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>',
    });
    
    async function cascade () {
      try {
        // After you resize the image to a height and width of 100 pixels, rotate the image 90°, and save the image to your local computer.
        let result = await client.get('<OSSObjectName>', '<LocalFileName>', {process: 'image/resize,m_fixed,w_100,h_100/rotate,90'});
      } catch (e) {
        console.log(e);
      }
    }
    cascade();
    ```


## Use image style to process images

You can encapsulate multiple IMG parameters in a style, and use the style to process multiple images at a time. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md). The following code provides an example on how to use image style to process an image:

```
const OSS = require('ali-oss');

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});

async function style () {
  try {
    // Use the specified image style to process the image and save the image to your local computer.
    let result = await client.get('<OSSObjectName>', '<LocalFileName>', {process: 'style/<yourCustomStyleName>'});
  } catch (e) {
    console.log(e);
  }
}
style();
```

## IMG persistence

The following code provides an example on how to save processed images:

```
const OSS = require('ali-oss');

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});
// The name of the source image. If the image is not stored in the root folder of the bucket, you must add the access path of the object. In this example, the path is example/example.jpg.
const sourceImage = '<SourceObjectName>';
// Specify the name of the processed image.
const targetImage = '<TargetObjectName>';
async function processImage(processStr, targetBucket) {
  const result = await client.processObjectSave(
    sourceImage,
    targetImage,
    processStr,
    targetBucket
  );
  console.log(result.res.status);
}

// Specify the IMG method and the bucket where the processed image is stored.
processImage("image/resize,m_fixed,w_100,h_100", "<target bucket>")
            
```

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
const OSS = require('ali-oss');

const client = new OSS({
  bucket: '<Your BucketName>',
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your Region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
});
// Generate a signed object URL that includes IMG parameters. Set the validity period to 10 minutes.
let signUrl = client.signatureUrl('<OSSObjectName>', {expires: 600, 'process' : 'image/resize,w_300'});
console.log("signUrl="+signUrl);
```

