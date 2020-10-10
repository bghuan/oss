# Alibaba Cloud storage service selection

Select appropriate Alibaba cloud storage services that best meet your requirements on data availability, data durability, and performance.

**Note:** Data availability indicates the ability of a storage service to provide data based on requests. Data durability indicates the annual average expected data loss of a storage service. Performance indicates the IOPS or throughput that a storage service can provide.

Alibaba Cloud provides the following three storage services to meet different requirements: Object Storage Service \(OSS\), Block Storage, and Apsara File Storage NAS. You can select the storage solution that best meets your requirements.

## OSS

OSS is a secure, cost-effective, and highly reliable cloud storage service provided by Alibaba Cloud. It is suitable to store unstructured data such as audio and video. OSS provides the highest level of data durability and availability among Alibaba Cloud storage services. OSS provides the following three storage classes: Standard, Infrequent Access \(IA\), and Archive. The three storage classes apply to hot data, warm data, and cold data respectively. The colder the data is, the lower the storage cost is, and the higher the cost for accessing the data is. You can easily convert the storage class of your data to optimize your storage costs.

## Block Storage

Block Storage is a high-performance, low-latency block storage service for Alibaba Cloud ECS. You can think of a Block Storage device as a physical disk. You can format a Block Storage device and create a file system on it.

Alibaba Cloud provides a variety of [Block Storage devices](https://ecs-buy.aliyun.com/?spm=5176.54360.880806.btn1.77b76a81ly4Be3#/clouddisk) for ECS instances, such as cloud disks based on a distributed storage architecture, and local disks located on the physical machines where the ECS instances are hosted. Cloud disks and local disks are described as follows:

-   Cloud disks are block-level storage devices provided by Alibaba Cloud for ECS instances. Cloud disks use a triplicate distributed mechanism and feature low latency, high performance, high durability, and high reliability. Cloud disks can be created, resized, and released at any time.
-   Local disks are physical disks attached to physical machines that host ECS instances. Local disks provide local storage access capabilities for ECS instances. Local disks are suitable for scenarios where high storage I/O performance and high cost performance for massive storage are required. Local disks feature low latency, high random IOPS and throughput, and high cost performance.

Cloud disks are billed based on their storage capacity. You can use cloud disks as system disks or data disks. Local disks are billed based on their storage capacity. You can use local disks only as data disks. Local disks cannot be purchased separately. Local disks created together with an ECS instance have the same billing method as the ECS instance. For more information about the product types and prices, see the [Block Storage pricing page](https://www.aliyun.com/price/product?spm=5176.54360.880806.btn1.19056a81lBSWXw#/disk/detail).

## Apsara File Storage NAS

Apsara File Storage NAS is a cloud service that provides file storage for compute nodes, including ECS instances, E-HPC nodes, and Alibaba Cloud Container Service for Kubernetes \(ACK\) nodes. Apsara File Storage NAS is a distributed file system that supports both the NFS and SMB protocols and features shared access, elastic scalability, high reliability, and high performance.

Apsara File Storage NAS provides the four storage types: Extreme NAS, NAS Performance, NAS Capacity, and Infrequent Access.

## Summary

OSS and Apsara File Storage NAS allocate storage resources based on the storage that you use. You only pay for the storage resources that you use. However, when you use Block Storage, you are charged for the pre-allocated storage resources regardless of whether you use the resources. Therefore, to maintain a low storage cost while meeting your requirements, it is important to use OSS to the maximum extent and use Block Storage with pre-configured I/O only when it is required for your application.

