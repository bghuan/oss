# Copy objects

This topic describes how to copy objects.

When you copy an object, there are two options to process the metadata of the object:

-   If the meta parameter is not specified, the metadata of the source object is copied.
-   If the meta parameter is specified, the metadata of the new object is used to overwrite the metadata of the source object.

The following code provides an example on how to copy objects from the same bucket:

```
let OSS = require('ali-oss');
var client = new OSS({
  bucket: 'bucket',
  region: 'oss-cn-xxx',
  accessKeyId: 'ak',
  accessKeySecret: 'aksecret',
// secure: true
})
// Copy the objects from the same bucket.
client.copy('test_copy', 'test').then((res) => {
  console.log(res);
}).catch(e => {
  console.log(e);
})
```

The following code provides an example on how to copy objects from two different buckets that are in the same region:

```
let OSS = require('ali-oss')

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>',
});

async function copy () {
  try {
     // When you copy objects from two different buckets that are in the same region, the name of the source object must be in the '/bucket/object' format.
    let result = await client.copy('to', '/from-bucket/from');
    console.log(result);

    // Copy the metadata.
    let result = await client.copy('to', 'from');
    console.log(result);

    // Overwrite the metadata.
    let result = await client.copy('to', 'from', {
      meta: {
        year: 2015,
        people: 'mary'
      }
    });
    console.log(result);
  } catch (e) {
    console.log(e);
  }
}
		
```

