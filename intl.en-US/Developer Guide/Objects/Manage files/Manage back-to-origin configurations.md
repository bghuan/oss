# Manage back-to-origin configurations

If you access data in a bucket that has no back-to-origin rules configured and the data does not exist, 404 Not Found is returned. However, if you configure back-to-origin rules that contain the correct origin URL, you can obtain the data based on the back-to-origin rules.

Back-to-origin supports the mirroring and redirection modes. You can configure back-to-origin rules for hot migration and specific request redirection. For more information about API operations used to configure back-to-origin rules, see [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md).

## Usage notes

-   You can configure up to 20 back-to-origin rules. Rules are implemented in a sequence that they are configured.
-   You cannot set origin URLs to internal endpoints.
-   Back-to-origin rules and versioning cannot be configured for a bucket at the same time.
-   In regions within China, the default queries per second \(QPS\) of mirroring-based back-to-origin is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin is 1,000, and the default bandwidth is 1 Gbit/s.
-   You can add Image Processing \(IMG\) parameters to origin URLs in mirroring-based back-to-origin, but cannot add video snapshot parameters.

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure back-to-origin rules.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/website.md)|A high-performance command-line tool|

## Mirroring-based back-to-origin

When you call the GetObject operation to query an object that does not exist in the bucket, OSS retrieves the object based on the origin URL. After OSS retrieves the object, OSS stores the object in the bucket and returns the objects to you. The following figure shows the detailed process.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6182364951/p1580.png)

-   Scenarios

    Mirroring-based back-to-origin is used to migrate data to OSS. This feature allows you to migrate any service that already runs on a user-created origin or in another cloud service to OSS without interrupting services. In this case, you can use mirroring-based back-to-origin to migrate data without stopping the service. For more information about the example, see [Seamlessly migrate data from a cloud service to OSS]().

-   Detail analysis
    -   Trigger conditions

        OSS performs mirroring-based back-to-origin to retrieve an object from the origin only when 404 is returned for GetObject.

    -   Naming conventions

        The URL used by OSS to obtain an object is `http(s)://MirrorURL+ObjectName`, which indicates the name of the requested object. For example, the origin URL configured for a bucket is `https://aliyun.com`, and the example.jpg requested object is not in the bucket. OSS obtains the object from `https://aliyun.com/example.jpg` and stores the object as example.jpg.

    -   Rules for failed back-to-origin requests

        If a requested object is not found in the origin, HTTP status code 404 is returned to OSS, and OSS returns the same status code to the user. If the origin returns a non-200 HTTP status code to indicate an error, such as object retrieval failures due to network-related errors, OSS returns HTTP status code `424 MirrorFailed` to users.

    -   x-oss-tag response header

        When OSS returns objects obtained by using mirroring-based back-to-origin, OSS adds the `x-oss-tag` response header and sets its value to `MIRROR + Space + url_decode (origin URL)`. Example:

        ```
        x-oss-tag:MIRROR http%3a%2f%2fwww.example-domain.com%2fdir1%2fimage%2fexample_object.jpg
        ```

        After OSS obtains an object from the origin, this header is added to the response when the object is downloaded. This header indicates that the object is obtained by using mirroring-based back-to-origin.

    -   Update rules of objects obtained by using mirroring-based back-to-origin

        After OSS obtains an object by using mirroring-based back-to-origin and the object is modified in the origin, OSS does not update the obtained object.

    -   Object metadata obtained by using mirroring-based back-to-origin

        OSS stores the following HTTP headers returned from the origin as the object metadata:

        ```
        Content-Type
                                        
        ```

    -   HTTP request rules
        -   OSS does not send the header information transmitted by a user to the origin. Whether OSS sends the QueryString information to the origin depends on the back-to-origin rules configured in the OSS console.
        -   If the origin returns chunked-encoded data, OSS also returns chunked-encoded data to the user.

## Redirection-based back-to-origin

Redirection-based back-to-origin enables OSS to return a redirect response and status code 3xx based on the specified back-to-origin conditions and corresponding redirection configurations. The following figure shows the detailed process.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6182364951/p1591.png)

Scenario:

-   Seamless data migration from third-party data sources to OSS

    When you use redirection-based back-to-origin to asynchronously migrate data from your data source to OSS, OSS returns a 302 redirect request by using URL rewrite for the data that is not migrated to OSS. Your client then reads the data from your data source based on the Location value in the 302 redirect request.

-   Page redirection

    For example, you can use redirection-based back-to-origin to hide objects whose names contain a specified prefix and return a specific page to visitors.

-   Page redirection for a 404 or 500 error

    You can use redirection-based back-to-origin to redirect users to a preset error page when a 404 or 500 error occurs. This method can prevent OSS from exposing system errors to users.


