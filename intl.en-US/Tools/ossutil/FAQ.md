# FAQ

This topic describes the problems you may encounter when you use ossutil and their solutions.

## What do I do if the skip message appears when the -u parameter is used to upload an object?

Analysis: When you use the -u parameter to upload an object, ossutil compares the object to upload with the existing objects in the bucket. When the name of the object to upload is the same as that of an existing object, if the last modified time of the object to upload is equal to or earlier than that of the existing object, the object is skipped. If the last modified time of the object to upload is later than that of the existing object, the object is uploaded. Therefore, the skip message is normal when you use the -u parameter to upload an object.

Solution: Ignore the message if the object is uploaded.

## What do I do if a 403 error is returned when an object is restored?

Analysis: This error is returned because of the following reasons:

-   The RAM user that you use to restore the object does not have corresponding permissions.
-   Access to the object is blocked because because its content is illegal.

Solution:

-   Grant permissions to the RAM user.
-   Delete or ignore the object.

## What do I do if an error is returned when a bucket that does not contain objects according to the result of the ls command is deleted?

Analysis: When you run the ls command without options included to list objects in a bucket, the parts and previous versions \(only for versioned buckets\) in the bucket are not listed. A bucket that contains parts or previous versions cannot be deleted by the rm command.

Solution:

-   Delete parts and previous versions \(only for versioned buckets\) in a bucket before you delete the bucket.
    1.  Delete parts and previous versions in a bucket.
        -   Run the following commands to list and delete parts in the bucket:

            ```
            ./ossutil ls oss://bucket1 -m
            ./ossutil rm -m oss://bucket1 -r
            ```

        -   Run the following commands to list and delete previous versions in the bucket:

            ```
            ./ossutil ls oss://bucket1 --all-versions
            ./ossutil rm oss://bucket1 --all-versions -r
            ```

    2.  Delete the bucket.

        ```
        ./ossutil rm oss://bucket1 -b
        ```

-   Force delete the bucket.
    -   Run the following command to force delete an unversioned bucket:

        ```
        ./ossutil rm oss://bucketname -abrf
        ```

    -   Run the following command to force delete a versioned bucket:

        ```
        ./ossutil rm oss://bucketname -abrf --all-versions
        ```


**Warning:** Deleted buckets and objects cannot be recovered. Exercise caution when you run the preceding commands.

## What do I do if the upload or download progress exceeds 100%?

Analysis: A folder named .ossutil\_checkpoint is generated when you use ossutil to upload or download objects. By default, if the object to upload or download is larger than 100 MB, ossutil uses resumable upload or download. The checkpoint file generated during the upload or download is stored in the .ossutil\_checkpoint folder. This folder is deleted after the upload or download task is completed. In scenarios where multiple ossutil instances are running on a computer at the same time and each instance is used to perform resumable upload or download, when the upload or download task in an instance is completed, the .ossutil\_checkpoint folder is deleted. In this case, the upload or download tasks in other instances cannot be completed and the progress exceeds 100%.

Solution:

-   Complete the current upload or download tasks and upload or download the object again.
-   Add the --checkpoint-dir parameter to the cp command and specify a folder whose name is different from the default checkpoint folder to store the checkpoint file. Example:

    ```
    ./ossutil cp oss://bucket1/myphoto.jpg  /dir  --checkpoint-dir checkpoint
    ```


## What do I do if multiple requests are recorded in OSS logs when you send a request to download a single object by using ossutil?

Analysis: This error is returned because of the following reasons:

-   ossutil retries the request because exceptions occur or the request fails. By default, ossutil retires a request for up to 10 times.
-   If the object you want to download is larger than 100 MB in size, ossutil sends multiple requests to obtain data by range. Parts of the object data in a range are obtained by each request until the whole object is downloaded. Therefore, multiple request records are generated when an object larger than 100 MB in size is downloaded.

