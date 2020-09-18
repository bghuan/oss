# Convert the storage classes of objects

OSS provides the following storage classes to cover various data storage scenarios from hot data to cold data: Standard, Infrequent Access \(IA\), Archive and Cold Archive. This topic describes how to convert the storage class of an object.

For more information about these storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md) and [Convert storage classes](/intl.en-US/Developer Guide/Storage classes/Convert storage classes.md) in the OSS Developer Guide.

The following section provides examples on how to convert the storage class of an object.

-   The following code provides an example on how to convert the storage class of an object from Standard or Infrequent Access \(IA\) to Archive:

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
        bucket: '<your bucket>',
        region: '<your region>',
        accessKeyId: '<your accessKeyId>',
        accessKeySecret: '<your accessKeySecret>'
    })
    var options = {
        headers:{'x-oss-storage-class':'Archive'}
    }
    client.copy('Objectname','Objectname',options).then((res) => {
        console.log(res);
    }).catch(err => {
        console.log(err)
    })
    ```

-   The following code provides an example on how to convert the storage class of an object from Archive to IA or Standard:

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
        bucket: '<your bucket>',
        region: '<your region>',
        accessKeyId: '<your accessKeyId>',
        accessKeySecret: '<your accessKeySecret>'
    })
    // The following code provides an example on how to convert the storage class of an object from Archive to IA:
    var options = {
        headers:{'x-oss-storage-class':'IA'}
    }
    client.copy('Objectname','Objectname',options).then((res) => {
        console.log(res);
    }).catch(err => {
        console.log(err)
    })
    ```


For more information about the storage classes and storage fees, see the [t4320.md\#section\_jmi\_2z4\_hka](/intl.en-US/Pricing/Billing items and methods/Overview.md) section in Billing items.

