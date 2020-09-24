# Benefits

OSS provides secure, cost-effective, and highly reliable services for you to store large amounts of data in the cloud. This topic compares OSS with the traditional user-created server storage to show the benefits of OSS.

## Advantages of OSS over user-created server storage

|Item|OSS|User-created server storage|
|:---|:--|---------------------------|
|Reliability|OSS is the core infrastructure of data storage for Alibaba Group. This service is highly available and reliable. OSS has provided support during peak hours of Double 11. OSS features a multi-redundant architecture, which provides reliable data storage. In addition, OSS is designed based on a high-availability architecture, which eliminates single point of failures \(SPOFs\) and ensures the continuity of data-based services. -   Provides 99.995% availability or service continuity \(designed for\).
-   Provides data durability of at least 99.9999999999% \(twelve 9s\).
-   Automatically expands capacities without affecting your services.
-   Automatically stores multiple copies of data for backup.

|-   Prone to errors due to low hardware reliability. If a disk has a bad sector, data may be lost.
-   Manual data restoration is complex and requires a lot of time and technical resources. |
|Security|-   Provides multi-level security protection for enterprises, including features such as server-side encryption, client-side encryption, hotlink protection, IP address blacklist or whitelist, fine-grained permission control, log audit, and Write Once Read Many \(WORM\) policies.
-   Provides resource isolation mechanisms for multiple tenants and supports zone-disaster recovery.
-   Obtains a number of compliance certifications, including certifications from institutions such as SEC and FINRA. This way, the data security and compliance requirements of enterprises can be met.

|-   Additional scrubbing devices and black hole policy-related services are required.
-   A separate security mechanism is required. |
|Cost|-   Provides multi-line BGP backbone networks with sufficient bandwidth resources. Upstream traffic is free of charge.
-   Requires no maintenance staff or hosting fees.

|-   Storage space is limited by hard disk capacity. The storage space must be manually resized.
-   Single-line or double-line access is slow, and bandwidth is limited. The bandwidth must be manually resized during peak hours.
-   Professional maintenance staff is required, which results in high costs. |
|Intelligent storage|Provides multiple data processing capabilities, such as Image Processing \(IMG\), video snapshot, document preview, image scenario recognition, facial recognition, and SQL in-place query. OSS seamlessly integrates with the Hadoop ecosystem and Alibaba Cloud services such as Function Compute, E-MapReduce, Data Lake Analytics, Batch Compute, MaxCompute, and Database Backup to manage data of enterprises.|Data processing capabilities must be purchased and separately deployed.|

## More benefits of OSS

-   Ease of use
    -   OSS provides standard RESTful API operations, a wide range of SDKs, client tools, and the OSS console. You can upload, download, retrieve, and manage large amounts of data for websites and mobile apps in the same way you use regular file systems.
    -   The capacity of each bucket is unlimited. Therefore, you can expand your buckets in OSS based on your requirements.
    -   Streaming writes and reads are supported, which is suitable for business scenarios where you must simultaneously read and write videos and other large objects.
    -   Lifecycle management is supported. You can configure lifecycle rules to delete multiple expired objects or convert the storage class of expired objects to cost-effective Infrequent Access \(IA\), Archive, or Cold Archive.
-   Powerful and flexible security mechanisms
    -   Flexible authentication and authorization mechanisms are available. OSS provides STS and URL authentication and authorization. OSS also supports IP address blacklists or whitelists, hotlink protection, and RAM.
    -   OSS provides user-level resource isolation. You can also use the multi-cluster synchronization service.
-   Data redundancy mechanism

    OSS uses a data redundant storage mechanism to store copies of each object on multiple devices of different facilities in the same region. This way, OSS ensures data reliability and availability if hardware failures occur.

    -   Operations on objects in OSS are highly consistent. For example, when you receive an upload or copy success response, the uploaded object can be immediately read, and the copies of the object are written to multiple devices for redundancy.
    -   To ensure complete data transmission, OSS checks for errors when packets are transmitted between the client and the server. To do so, OSS calculates the checksum of the network traffic packets.
    -   The data redundancy mechanism of OSS can prevent data loss when two storage devices are damaged at the same time.
        -   After data is stored in OSS, OSS regularly checks whether copies of the data are lost. Then, OSS recovers lost copies to ensure the reliability and availability of data.
        -   OSS periodically verifies the integrity of data to detect data corruption caused by errors such as hardware failures. If data is partially corrupted or lost, OSS reconstructs and repairs the corrupted data by using the other copies.
-   Rich and powerful value-added services
    -   IMG: supports format conversion, thumbnails, cropping, watermarking, resizing for objects in formats such as JPG, PNG, BMP, GIF, WebP, and TIFF.
    -   Audio or video transcoding: provides high-quality, high-speed, and parallel audio or video transcoding capabilities. This way, your audio or video files can be played on different terminal devices.
    -   Accelerated Internet access: provides transfer acceleration that accelerates uploads and downloads across provinces and continents and improves user experience. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).
    -   Accelerated content delivery: uses OSS as the origin with CDN to improve user experience when the same object is repeatedly downloaded.

