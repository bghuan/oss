# Data retention compliance

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time. This strategy is applicable to business under the regulations of the U.S. Securities and Exchange Commission \(SEC\) and Financial Industry Regulatory Authority, Inc. \(FINRA\).

OSS is the only cloud service in China that has passed the audit and certification of Cohasset Associates and can meet specific requirements for electronic data storage. OSS buckets configured with retention policies can be used for business that is subject to regulations such as SEC Rule 17a-4\(f\), CFTC Rule 1.31\(c\)-\(d\), and FINRA Rule 4511\(c\). For more information, see [OSS Cohasset Assessment](http://gosspublic.alicdn.com/OSSCohassetAssessmentReport.pdf).

OSS provides strong compliant policies. You can configure time-based retention policies for buckets. After a retention policy is locked, you can read objects from or upload objects to buckets. However, the objects or retention policies within the retention period cannot be deleted. You can delete objects only after their retention period ends. The WORM strategy is suitable for industries such as finance, insurance, health care, and securities. OSS provides the WORM strategy to allow you to build a compliant bucket in the cloud.

For more information, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

