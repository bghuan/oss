# Retention policy

You can configure a time-based retention policy for a bucket. A retention policy has a retention period that ranges from one day to 70 years. This topic describes how to create, query, and lock a retention policy.

## Background information

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time.

If a retention policy is not locked within 24 hours after it is created, the retention policy becomes invalid. After the retention policy configured for a bucket is locked, you can upload objects to or read objects from the bucket. However, you cannot delete objects in the bucket or the retention policy within the retention period of the policy. The retention period of the policy cannot be shortened but only be extended. For more information about retention policies, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

## Create a retention policy

The following code provides an example on how to create a retention policy:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
// Create the retention policy.
async function initiateBucketWorm() {
  const bucket = '<Your Bucket Name>'
  // Specify the retention period of the retention policy.
  const days = '<Retention Days>'
	const res = await client.initiateBucketWorm(bucket, days)
  console.log(res.wormId)
}

initiateBucketWorm()
```

For more information about how to create a retention policy, see [InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md).

## Cancel an unlocked retention policy

The following code provides an example on how to cancel an unlocked retention policy:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
// Cancel the retention policy.
async function abortBucketWorm() {
  const bucket = '<Your Bucket Name>'
	try {
    await client.abortBucketWorm(bucket)
    console.log('abort success')
  } catch(err) {
		console.log('err: ', err)
	}
}

abortBucketWorm()
```

For more information about how to cancel an unlocked retention policy, see [AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md).

## Lock a retention policy

The following code provides an example on how to lock a retention policy:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
// Lock the retention policy.
async function completeBucketWorm() {
  const bucket = '<Your Bucket Name>'
  const wormId = '<Your Worm Id>'
	try {
		await client.completeBucketWorm(bucket, wormId)
  } catch(err) {
  	console.log(err)
  }
}

completeBucketWorm()
```

For more information about how to lock a retention policy, see [CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md).

## Query a retention policy

The following code provides an example on how to query a retention policy:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
// Query the retention policy.
async function getBucketWorm() {
  const bucket = '<Your Bucket Name>'
	try {
		const res = await client.getBucketWorm(bucket)
    // Query the ID of the retention policy.
    console.log(res.wormId)
    // Query the status of the retention policy. InProgress indicates that the retention policy is not locked. Locked indicates that the retention policy is locked.
    console.log(res.state)
    // Query the retention period of the retention policy.
    console.log(res.days)
  } catch(err) {
  	console.log(err)
  }
}

getBucketWorm()
```

For more information about how to query a retention policy, see [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md).

## Extend the retention period of a retention policy

The following code provides an example on how to extend the retention period of a locked retention policy:

```
const OSS = require('ali-oss');

const client = new OSS({
  // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
  region: '<Your region>',
  // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
// Extend the retention period of the locked retention policy.
async function extendBucketWorm() {
  const bucket = '<Your Bucket Name>'
  const wormId = '<Your Worm Id>'
  const days = '<Retention Days>'
	try {
		const res = await client.extendBucketWorm(bucket, wormId, days)
    console.log(res)
  } catch(err) {
  	console.log(err)
  }
}

extendBucketWorm()
```

For more information about how to extend the retention period of a locked retention policy, see [ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md).

