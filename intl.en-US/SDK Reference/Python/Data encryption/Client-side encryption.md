# Client-side encryption

Client-side encryption is used to encrypt objects on the local client before they are uploaded to OSS.

## Disclaimer

-   When you use client-side encryption, you must ensure the integrity and validity of the Customer Master Key \(CMK\). If the CMK is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.
-   When you copy or migrate encrypted data, you must ensure the integrity and validity of the object metadata related to client-side encryption. If the object metadata related to client-side encryption is used incorrectly or lost due to improper maintenance, you will be liable for any losses or consequences caused by decryption failures.

## Background information

In client-side encryption, a random data key is generated for each object to perform symmetric encryption on the object. The client uses a CMK to encrypt the random data key. The encrypted data key is uploaded as a part of the object metadata and stored in the OSS server. When an encrypted object is downloaded, the client uses the CMK to decrypt the random data key and then uses the data key to decrypt the object. The CMK is used only on the client and is not transmitted over the network or stored in the server, which ensures data security.

## Encryption methods

You can use CMKs managed in one of the following ways:

-   Use CMKs managed by Key Management Service \(KMS\)

    When you use a CMK stored in KMS for client-side encryption, you must send the CMK ID to OSS SDK.

-   Use CMKs managed by yourself \(RSA\)

    When you use a CMK managed by yourself for client-side encryption, you must send the public key and the private key of your CMK to OSS SDK as parameters.


You can use the preceding methods to prevent data leaks and protect your data on the client. Even if your data is leaked, it cannot be decrypted by others.

For more information about client-side encryption, see [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md) in the Developer Guide. For the complete sample code, visit [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_crypto.py).

## Client-side encryption V2 \(recommended\)

**Note:**

-   Client-side encryption V2 supports multipart upload for objects larger than 5 GB. When you use multipart upload to upload an object, you must specify the total size of the object and the size of each part. The size of each part except for the last part must be the same and be a multiple of 16 bytes.
-   After you upload objects encrypted on the client, the object metadata related to client-side encryption is protected and you cannot call operations such as [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) to modify the metadata related to client-side encryption.

-   Object metadata related to client-side encryption

    |Parameter|Description|Required|
    |---------|-----------|--------|
    |x-oss-meta-client-side-encryption-key|The encrypted data key. The encrypted data key is a string encrypted by using a CMK managed by RSA or KMS and encoded in Base64.|Yes|
    |x-oss-meta-client-side-encryption-start|The initialization vector generated randomly for data encryption. The initialization vector is a string encrypted by using a CMK managed by RSA or KMS and encoded in Base64.|Yes|
    |x-oss-meta-client-side-encryption-cek-alg|The encryption algorithm used to encrypt data.|Yes|
    |x-oss-meta-client-side-encryption-wrap-alg|The encryption algorithm used to encrypt data keys.|Yes|
    |x-oss-meta-client-side-encryption-matdesc|The description of the content encryption key \(CEK\) in the JSON format.|No|
    |x-oss-meta-client-side-encryption-unencrypted-content-length|The length of data before encryption. This parameter is not generated if content-length is not specified.|No|
    |x-oss-meta-client-side-encryption-unencrypted-content-md5|The MD5 hash of data before encryption. This parameter is not generated if Content-MD5 is not specified.|No|
    |x-oss-meta-client-side-encryption-data-size|The total size of the object to upload in multipart upload.|No \(required for multipart upload\)|
    |x-oss-meta-client-side-encryption-part-size|The size of each part in multipart upload.|No \(required for multipart upload\)|

-   Create a bucket for client-side encryption

    Before you encrypt an object to upload or decrypt a downloaded object on the client, you must initialize a bucket instance. You can call the operations of the bucket instance to upload or download objects. In client-side encryption, the CryptoBucket class inherits the operations from the Bucket class. You can specify corresponding parameters to initialize a CrytoBucket instance in the same way as a bucket instance.

    -   Initialize a bucket for RSA-based encryption

        **Note:** If you use RSA, you must manage the CMK by yourself. The loss of CMK or the damage to CMK data may cause decryption failures. We recommend that you use CMKs managed by KMS. If you must use RSA to perform encryption, we recommend that you back up your CMK data.

        The following code provides an example on how to initialize a bucket for RSA-based encryption:

        ```
        # -*- coding: utf-8 -*-
        import os
        import oss2
        from  oss2.crypto import RsaProvider
        
        # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
        auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
        
        key_pair = {'private_key': '<yourPrivateKey>', 'public_key': '<yourPublicKey>'}
        bucket = oss2.CryptoBucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name,
                                   crypto_provider=RsaProvider(key_pair))
        ```

    -   Initialize a bucket for KMS-based encryption

        The following code provides an example on how to initialize a bucket for KMS-based encryption:

        ```
        import os
        
        import oss2
        from oss2.crypto import AliKMSProvider
        
        kms_provider=AliKMSProvider('<yourAccessKeyId>', '<yourAccessKeySecret>', '<yourRegion>', '<yourCMKID>')
        bucket = oss2.CryptoBucket(oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>'), '<yourEndpoint>', '<yourBucket>', crypto_provider = kms_provider)
        ```

    -   Use and manage multiple CMKs

        You can use different CMKs to encrypt objects to upload to a bucket or decrypt objects downloaded from a bucket. You can configure different descriptions for the CMKs and add the CMKs and their descriptions to the encryption-related information about the bucket. When data is decrypted, OSS SDK automatically matches a CMK based on the description for seamless decryption. The following code provides an example on how to use and manage multiple CMKs:

        ```
        # -*- coding: utf-8 -*-
        import os
        import oss2
        from  oss2.crypto import RsaProvider
        
        # Create an RSA key pair.
        key_pair_1 = {'private_key': '<yourPrivateKey_1>', 'public_key': '<yourPublicKey_1>'}
        mat_desc_1 = {'key1': 'value1'}
        
        # Create another RSA key pair.
        key_pair_2 = {'private_key': '<yourPrivateKey_2>', 'public_key': '<yourPublicKey_2>'}
        mat_desc_2 = {'key2': 'value2'}
        
        # Add the description of key_pair_1 to provider.
        provider = RsaProvider(key_pair={'private_key': private_key_str_2, 'public_key': public_key_str_2}, mat_desc=mat_desc_2)
        encryption_materials = oss2.EncryptionMaterials(mat_desc_1, key_pair=key_pair_1)
        provider.add_encryption_materials(encryption_materials)
        
        # Use provider to initialize a crypto_bucket instance. Then, you can use the operations of crypto_bucket to download the object data that is encrypted by a CMK whose description is mat_desc_1.
        crypto_bucket = oss2.CryptoBucket(oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>'), '<yourEndpoint>', '<yourBucketName>', crypto_provider=provider)
        ```

-   Perform KMS-based client-side encryption in simple upload and download

    The following code provides an example on how to use a CMK stored in KMS to encrypt objects in simple upload or decrypt objects in simple download:

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import RsaProvider
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    kms_provider=AliKMSProvider('<yourAccessKeyId>', '<yourAccessKeySecret>', '<yourRegion>', '<yourCMKID>')
    bucket = oss2.CryptoBucket(oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>'), '<yourEndpoint>', '<yourBucket>', crypto_provider = kms_provider)
    
    key = 'motto.txt'
    content = b'a' * 1024 * 1024
    filename = 'download.txt'
    
    
    # Upload the object.
    bucket.put_object(key, content, headers={'content-length': str(1024 * 1024)})
    
    # Download the object.
    result = bucket.get_object(key)
    
    # Verify the consistency between the content of the downloaded object and that of the object before upload.
    content_got = b''
    for chunk in result:
        content_got += chunk
    assert content_got == content
    
    # Download the object to a local file.
    result = bucket.get_object_to_file(key, filename)
    
    # Verify the consistency between the content of the downloaded object in the local file and that of the object before upload.
    with open(filename, 'rb') as fileobj:
        assert fileobj.read() == content
    ```

-   Perform KMS-based client-side encryption in multipart upload

    **Note:**

    -   During multipart upload, if the upload task is interrupted and the process exists, the context for multipart upload may be lost. If a multipart upload task is interrupted and you want to upload the object again, you must upload the entire object again.
    -   We recommend that you use the operation of resumable upload in OSS to upload large objects. By using resumable upload, you can store the context of your multipart upload in the local client so that it will be retained even if the upload task is interrupted.
    The following code provides an example on how to use a CMK stored in KMS to encrypt an object to upload by using multipart upload:

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import RsaProvider
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    kms_provider=AliKMSProvider('<yourAccessKeyId>', '<yourAccessKeySecret>', '<yourRegion>', '<yourCMKID>')
    bucket = oss2.CryptoBucket(oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>'), '<yourEndpoint>', '<yourBucket>', crypto_provider = kms_provider)
    
    """
    Multipart upload
    """
    # Initialize the parts to upload.
    part_a = b'a' * 1024 * 100
    part_b = b'b' * 1024 * 100
    part_c = b'c' * 1024 * 100
    multi_content = [part_a, part_b, part_c]
    
    parts = []
    data_size = 100 * 1024 * 3
    part_size = 100 * 1024
    multi_key = "test_crypto_multipart"
    
    # Initialize the context for multipart upload when you use client-side encryption.
    context = models.MultipartUploadCryptoContext(data_size, part_size)
    res = bucket.init_multipart_upload(multi_key, upload_context=context)
    upload_id = res.upload_id
    
    # In this example, parts are uploaded sequentially. In practice, multipart upload supports multi-threaded upload to speed up the upload.
    for i in range(3):
        # The values in context cannot be modified. If the values in context are modified, data upload fails.
        result = bucket.upload_part(multi_key, upload_id, i + 1, multi_content[i], upload_context=context)
        parts.append(oss2.models.PartInfo(i + 1, result.etag, size=part_size, part_crc=result.crc))
    
    # Complete multipart upload.
    result = bucket.complete_multipart_upload(multi_key, upload_id, parts)
    
    # Verify the consistency between the content of the downloaded object and that of the object before upload.
    result = bucket.get_object(multi_key)
    content_got = b''
    for chunk in result:
        content_got += chunk
    assert content_got[0:102400] == part_a
    assert content_got[102400:204800] == part_b
    assert content_got[204800:307200] == part_c
    ```

-   Perform RSA-based client-side encryption in resumable upload

    The following code provides an example on how to use a CMK managed by yourself to encrypt objects in resumable upload:

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import RsaProvider
    
    key = 'motto.txt'
    content = b'a' * 1024 * 1024 * 100
    file_name_put = 'upload.txt'
    
    # Write the data included in content to the file.
    with open(file_name_put, 'wb') as fileobj:
        fileobj.write(content)
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to the RAM console.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # Create a bucket and use CMKs managed by yourself (RSA) to perform client-side encryption. This encryption method supports only uploads and downloads of entire objects.
    # If you use OSS SDK for Python V2.9.0 or later, we recommend that you do not use LocalRsaProvider to initialize the bucket.
    # bucket = oss2.CryptoBucket(auth,'<yourEndpoint>', '<yourBucketName>', crypto_provider=LocalRsaProvider())
    
    key_pair = {'private_key': '<yourPrivateKey>', 'public_key': '<yourPublicKey>'}
    
    # Initializes the bucket. Set upload_contexts_flag to True, and you do not need to specify parameters for multipart upload context when you call the upload_part operation.
    bucket = oss2.CryptoBucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name,
                               crypto_provider=RsaProvider(key_pair))
    
    In this example, the value of multipart_threshold is set to 10 * 1024 * 1024 for convenience. In practice, you can set the value of multipart_threshold as needed. Default value: 10 MB.
    # multipart_threshold indicates a threshold for objects sizes. If the object size exceeds the threshold, multipart upload is used. If the object size is smaller than the threshold, we recommend that you use the put_object operation to upload the object.
    # part_size specifies the size of each part in multipart upload. Default value: 10 MB.
    # num_threads specifies the number of concurrent upload threads. Default value: 1.
    oss2.resumable_upload(bucket, key, file_name_put, multipart_threshold=10 * 1024 * 1024, part_size=1024 * 1024, num_threads=3)
    ```

-   Perform KMS-based client-side encryption in resumable download

    The following code provides an example on how to use a CMK stored in KMS to decrypt objects in resumable download:

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import RsaProvider
    
    key = 'motto.txt'
    content = b'a' * 1024 * 1024 * 100
    file_name_get = 'download.txt'
    
    kms_provider=AliKMSProvider('<yourAccessKeyId>', '<yourAccessKeySecret>', '<yourRegion>', '<yourCMKID>')
    bucket = oss2.CryptoBucket(oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>'), '<yourEndpoint>', '<yourBucket>', crypto_provider = kms_provider)
    
    
    # Resumable download.
    oss2.resumable_download(bucket, key, file_name_get, multiget_threshold=10 * 1024 * 1024, part_size=1024 * 1024, num_threads=3)
    
    # Verify the consistency between the content of the downloaded object and that of the object before upload.
    with open(file_name_get, 'rb') as fileobj:
        assert fileobj.read() == content
    ```


## Client-side encryption V1 \(not recommended\)

**Note:**

-   Client-side encryption V1 supports only the upload of objects smaller than 5 GB by using PutObject. The multipart upload, resumable upload and resumable download operations are not supported.
-   After you upload an object by using the operations of the CryptoBucket class, you cannot call operations such as CopyObject to modify the metadata related to client-side encryption. If the metadata is modified, the data may fail to be decrypted.
-   Only OSS SDK for Python supports client-side encryption V1. OSS SDK for other languages cannot decrypt data uploaded in client-side encryption V1.
-   OSS SDK for Python V2.11.0 and later supports client-side encryption V2. Client-side encryption V2 supports comprehensive features. We recommend that you upgrade OSS SDK to the latest version.

-   Object metadata related to client-side encryption

    |Parameter|Description|Required|
    |:--------|:----------|:-------|
    |x-oss-meta-oss-crypto-key|The encrypted data key. The encrypted data key is a string encrypted by using a CMK managed by RSA and encoded in Base64.|Yes|
    |x-oss-meta-oss-crypto-start|The initial value generated randomly for data encryption. The initial value is a string encrypted by using a CMK managed by RSA and encoded in Base64.|Yes|
    |x-oss-meta-oss-cek-alg|The encryption algorithm used to encrypt data.|Yes|
    |x-oss-meta-oss-wrap-alg|The encryption algorithm used to encrypt data keys.|Yes|
    |x-oss-meta-oss-matdesc|The description of the CEK in the JSON format. This parameter does not take effect currently.|No|
    |x-oss-meta-unencrypted-content-length|The length of data before encryption. This parameter is not generated if content-length is not specified.|No|
    |x-oss-meta-unencrypted-content-md5|The MD5 hash of data before encryption. This parameter is not generated if Content-MD5 is not specified.|No|

-   Perform RSA-based client-side encryption in object upload and download

    The following code provides an example on how to use a CMK managed by yourself to encrypt an object to upload or decrypt an object to download:

    ```
    # -*- coding: utf-8 -*-
    import os
    import oss2
    from  oss2.crypto import LocalRsaProvider
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to https://ram.console.aliyun.com.
    auth = oss2.Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # Create a bucket and use CMKs managed by yourself (RSA) to perform client-side encryption. This encryption method supports only uploads and downloads of entire objects.
    bucket = oss2.CryptoBucket(auth,'<yourEndpoint>', '<yourBucketName>', crypto_provider=LocalRsaProvider())
    
    key = 'motto.txt'
    content = b'a' * 1024 * 1024
    filename = 'download.txt'
    
    
    # Upload the object.
    bucket.put_object(key, content, headers={'content-length': str(1024 * 1024)})
    
    # Download the object.
    result = bucket.get_object(key)
    
    # Verify the consistency between the content of the downloaded object and that of the object before upload.
    content_got = b''
    for chunk in result:
        content_got += chunk
    assert content_got == content
    
    # Download the object to a local file.
    result = bucket.get_object_to_file(key, filename)
    
    # Verify the consistency between the content of the downloaded object and that of the object before upload.
    with open(filename, 'rb') as fileobj:
        assert fileobj.read() == content
    ```


