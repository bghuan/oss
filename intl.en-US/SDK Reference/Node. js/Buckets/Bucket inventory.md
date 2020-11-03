Bucket inventory 
=====================================

This topic describes how to add inventory rules for a bucket and how to query, list, and delete inventory rules configured for a bucket.
**Notice**

Ensure that you have permissions to perform the preceding operations. By default, the bucket owner have permissions to perform these operations. If you do not have permissions to perform these operations, apply for the permissions from the bucket owner.

Add inventory rules 
----------------------------------------

Take note of the following items when you add inventory rules for a bucket:

* You can add up to 1,000 inventory rules for a bucket.

  

* The inventory list must be stored in a bucket within the same region as the bucket for which the inventory is configured.

  




The following code provides an example on how to add inventory rules for a bucket:

    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<oss-cn-hangzhou>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    const inventory = {
      // Specify the ID of the inventory rule.
      id: 'default', 
      // Specify whether inventory is enabled for the bucket. Valid values: true and false.
      isEnabled: false, 
      // (Optional) Specify the rule used to filter the objects to include in inventory lists. In this example, prefix is used to filter the objects to include in inventory lists.
      prefix: 'ttt',
      OSSBucketDestination: {
         // Specify the format of the inventory lists.
        format: 'CSV',
       // Specify the account ID of the owner of the destination bucket where the generated inventory lists are stored.
        accountId: '<Your AccountId>', 
       // Specify the role used to access the destination bucket where the generated inventory lists are stored.
        rolename: 'AliyunOSSRole',
        // Specify the name of the destination bucket where the generated inventory lists are stored.
        bucket: '<Your BucketName>',
        // (Optional) Specify the folder where the generated inventory lists are stored.
        prefix: '<Your Prefix>',
        // To encrypt inventory lists by using keys managed by OSS, use the following code.
        //encryption: {'SSE-OSS': ''},
        // To encrypt inventory lists by using CMKs managed by KMS, use the following code.
       	/*
        	encryption: {
          'SSE-KMS': {
            keyId: 'test-kms-id';
          };, 
        */
      },
      // Specify the frequency at which inventory lists are generated. A value of Weekly indicates that an inventory list is generated each week. A value of Daily indicates that an inventory list is generated each day.
      frequency: 'Daily', 
      // Specify that all versions of the objects are included in inventory lists. If the value of includedObjectVersions is set to Current, only the current versions of the objects are included in inventory lists.
      includedObjectVersions: 'All', 
      optionalFields: {
        // (Optional) Specify object attributes that are included in inventory lists.
        field: ["Size", "LastModifiedDate", "ETag", "StorageClass", "IsMultipartUploaded", "EncryptionStatus"]
      },
    }
    
    async function putInventory(){
      // Specify the name of the bucket for which you want to add inventory rules.
      const bucket = '<Your BucketName>'; 
    	try {
        await client.putBucketInventory(bucket, inventory);
        console.log('The inventory rule is added.')
      } catch(err) {
        console.log('Failed to add the inventory rule: ', err);
      }
    }
    
    putInventory()



For more information about how to add inventory rules for a bucket, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

Query an inventory rule 
--------------------------------------------

The following code provides an example on how to query an inventory rule configured for a bucket:

    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<oss-cn-hangzhou>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    async function getBucketInventoryById() {
      // Specify the name of the bucket from which you want to query the inventory rule.
      const bucket = '<Your BucketName>';
      try {
        // Query the inventory rule configured for the bucket.
        const result = await client.getBucketInventory(bucket, 'inventoryid');
       	console.log(result.inventory);
      } catch (err) {
       	console.log(err)
      }
    }
    
    getBucketInventoryById();



For more information about how to query an inventory rule configured for a bucket, see [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md).

List inventory rules 
-----------------------------------------

**Note**

You can query up to 100 inventory rules by sending a request. To query more than 100 inventory rules, you must send multiple requests and use the token returned for each request as the parameter for the next request.

The following code provides an example on how to list all inventory rules configured for a bucket at a time:

    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<oss-cn-hangzhou>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    async function listBucketInventory() {
      const bucket = '<Your Bucket Name>';
      // List the inventory rules configured for the bucket.
      const result = await client.listBucketInventory(bucket);
      console.log(result.inventoryList);
      // By default, 100 inventory rules are listed each time. If more the 100 inventory rules are configured for the bucket, the results are listed in multiple pages. You must use the nextContinuationToken returned for the request to send the next request to list the next page.
      const { nextContinuationToken } = result;
      const resultNext = await client.listBucketInventory(bucket, nextContinuationToken);
      console.log(result.inventoryList);
    }
    
    listBucketInventory();



For more information about how to list multiple inventory rules at a time, see [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md).

Delete an inventory rule 
---------------------------------------------

The following code provides an example on how to delete an inventory rule configured for a bucket:

    const OSS = require('ali-oss');
    
    const client = new OSS({
      bucket: '<Your BucketName>',
      // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
      region: '<oss-cn-hangzhou>',
      // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
      accessKeyId: '<Your AccessKeyId>',
      accessKeySecret: '<Your AccessKeySecret>'
    });
    
    async function deleteBucketInventoryById() {
      const bucket = '<Your Bucket Name>';
      // Delete the inventory rule with the specified ID.
      const inventory_id = '<Your InventoryId>'; 
    	try {
        await client.deleteBucketInventory(bucket, inventory_id);
      } catch (err) {
        console.log(err)
      }
    }
    deleteBucketInventoryById();



For more information about how to delete an inventory rule configured for a bucket, see [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md).
