# Overview

OSS provides the following storage classes to cover various data storage scenarios from hot data storage to cold data storage: Standard, Infrequent Access \(IA\), and Archive, and Cold Archive.

## Standard

Standard provides highly reliable, highly available, and high-performance object storage services that can handle frequent data access. Standard is suitable to store images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Standard includes the following data redundancy storage mechanisms:

-   Locally redundant storage \(LRS\)

    If you specify LRS, OSS stores the copies of each object on multiple devices of different facilities in the same region. This way, OSS ensures data reliability and availability in case of hardware failures.

-   Zone-redundant storage \(ZRS\)

    ZRS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data is still accessible.


## IA

IA provides highly reliable object storage services at prices lower than Standard. Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in real time. You are charged for the data retrieval. IA applies to scenarios where stored data is infrequently accessed \(once or twice each month\). IA includes the following data redundancy storage mechanisms:

-   LRS

    If you specify LRS, OSS stores the copies of each object on multiple devices of different facilities in the same region. This way, OSS ensures data reliability and availability in case of hardware failures.

-   ZRS

    ZRS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data is still accessible.


## Archive

Archive provides highly reliable object storage services at prices lower than Standard and IA. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. You must restore an object of the Archive storage class before you can access it. The restoration takes about one minute, and you are charged for the data retrieval. Archive is suitable for data that you want to store for a long period of time such as archival data, medical images, scientific materials, and video footage.

## Cold Archive

Cold Archive provides highly reliable object storage services at prices lower than Standard, IA, and Archive. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. You must restore an object of the Cold Archive storage class before you can access it. The time required to restore an Cold Archive object depends on the object size and restore mode. You are charged for the data retrieval when you restore a Cold Archive object. Cold Archive is suitable to store extremely cold data that you want to store for an ultra-long period. Such data includes data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, media resources that are retained in the film and television industries, and archived videos from the online education industry.

**Note:** The Cold Archive storage class is in public preview in the Australia \(Sydney\), US \(Silicon Valley\), Germany \(Frankfurt\), Indonesia \(Jakarta\), India \(Mumbai\), China \(Hong Kong\), and Singapore region. You can contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

## Comparison of storage classes

|Item|Standard LRS|Standard ZRS|IA LRS|IA ZRS|Archive|Cold Archive|
|:---|:-----------|------------|------|:-----|:------|------------|
|Data durability \(designed for\)|99.999999999% \(eleven 9's\)|99.9999999999% \(twelve 9's\)|99.999999999% \(eleven 9's\)|99.9999999999% \(twelve 9's\)|99.999999999% \(eleven 9's\)|99.999999999% \(eleven 9's\)|
|Service availability \(designed for\)|99.99%|99.995%|99.99%|99.995%|99.99% \(restored data\)|99.99% \(restored data\)|
|Service availability|99.90%|99.95%|99.00%|99.50%|99.00% \(restored data\)|99.00% \(restored data\)|
|Minimum billable size of objects|None|None|64 KB|64 KB|64 KB|64 KB|
|Minimum storage period|None|None|30 days|30 days|60 days|180 days|
|Data retrieval fees|None|None|Based on the size of retrieved data. Unit: GB.|Based on the size of retrieved data. Unit: GB.|Based on the size of restored data. Unit: GB.|Based on the size of restored data and the data retrieval capability that is selected. Unit: GB.|
|Data access|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Real-time access and low latency within milliseconds|Supported after data is restored. It takes one minute for data to be restored.|You must restore a Cold Archive object before you can read it. The time required to restore a Cold Archive object to the readable state is determined based on the restoration priority of the object: -   Expedited: The object is restored within one hour.
-   Standard: The object is restored within two to five hours.
-   Batch: The object is restored within 5 to 12 hours. |
|Image Processing \(IMG\)|Supported.|Supported.|Supported.|Supported.|Supported after data is restored.|Supported after data is restored.|
|Scenario|Suitable to store images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Examples: program download and mobile applications.|Suitable to store images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics. Standard ZRS applies to scenarios where higher availability and reliability are required. Examples: important enterprise documents and sensitive information.|Suitable to store data that is infrequently accessed \(once or twice each month\). Examples: hot backup data and surveillance video data.|Suitable to store data that is infrequently accessed \(once or twice each month\). IA ZRS applies to scenarios where higher availability and reliability are required. Examples: enterprise business data and recent medical records.|Suitable to store data that you want to store for a long period of time. Examples: archival data, medical images, scientific materials, and video footage.|Suitable to store extremely cold data that you want to store for an ultra-long period of time. Examples: data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, media resources that are retained in the film and television industries, and archived videos from the online education industry.|

**Note:** OSS charges data retrieval fees based on the amount of data read from the underlying distributed storage system. Data transmitted over the Internet is billed as outbound traffic.

