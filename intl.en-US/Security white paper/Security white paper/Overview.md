# Overview

Alibaba Cloud Object Storage Service \(OSS\) provides rich security capabilities and supports various security features, including server-side encryption, client-side encryption, hotlink protection based on Referer whitelists, fine-grained access control, log audit, and retention policies based on WORM. OSS provides complete security protection for your data stored in Alibaba Cloud to meet your security and compliance requirements on enterprise data.

OSS is the only cloud service in China that has passed the audit and certification of Cohasset Associates and can meet specific requirements for electronic data storage. OSS buckets configured with retention policies can be used for business that is subject to regulations such as SEC Rule 17a-4\(f\), CFTC Rule 1.31\(c\)-\(d\), and FINRA Rule 4511\(c\). In addition, OSS has passed the following compliance certification:

-   ISO9001, ISO20000, ISO22301, ISO27001, ISO27017, ISO27018, ISO29151, and ISO27701
-   BS10012
-   CSA STAR
-   PCI DSS
-   C5
-   MTCS
-   GxP
-   TPN
-   Trusted cloud service authentication
-   SOC 1/2/3 reports

This topic describes the security capabilities provided by OSS, including the following features:

|[Access control]()|OSS provides access control list \(ACL\), RAM and bucket policies, and hotlink protection based on Referer whitelists to control and manage access to your OSS resources.|
|[Data encryption]()|OSS provides server-side encryption, client-side encryption, and encrypted transmission based on SSL or TLS to protect data from potential security risks on the cloud.|
|[Monitoring and audit]()|OSS allows you to store and query access logs to meet your requirements on the monitoring and audit of enterprise data.|
|[Disaster recovery]()|OSS supports zone-redundant storage and cross-region replication to provide disaster recovery capabilities for data centers in a same region or across multiple regions.|
|[Data retention compliance]()|OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time. This strategy is applicable to business under the regulations of the U.S. Securities and Exchange Commission \(SEC\) and Financial Industry Regulatory Authority, Inc. \(FINRA\).|
|[Other features]()|OSS provides versioning to prevent data from being accidentally deleted or overwritten. If your bucket is attacked or used to share illegal content, OSS moves the bucket to a sandbox to prevent your other buckets from being affected.|

