# Monitoring and auditing

OSS provides logging and real-time log query and supports Inner-ActionTrail for buckets to address your monitoring and auditing needs for enterprise data.

## Logging

When you access OSS, a large number of access logs are generated. After you enable and configure logging for a bucket, OSS generates an object based on the predefined naming conventions. Access logs are generated on an hourly basis and written to the specified bucket as objects. You can configure lifecycle rules for the specified destination bucket to convert the storage class of the log objects to Archive. This way, these log objects can be retained for a long time. For more information, see [Logging](/intl.en-US/Developer Guide/Manage logs/Logging.md) in OSS Developer Guide.

## Real-time log query

OSS uses Log Service to support real-time log query. In the OSS console, you can query and collect statistics for access logs and audit access in OSS, track exception events, and troubleshoot problems. Real-time log query helps you improve work efficiency and make informed decisions. For more information, see [Real-time log query](/intl.en-US/Developer Guide/Manage logs/Real-time log query.md) in OSS Developer Guide.

## Inner-ActionTrail

Alibaba Cloud ActionTrail provides the Inner-ActionTrail feature. This feature allows you to transfer operations logs of Alibaba Cloud services from ActionTrail to Log Service in near real time for analysis and auditing. ActionTrail records and stores the operations logs of Alibaba Cloud services. You can transfer these logs to Log Service for analysis and auditing based on the search and analysis, reporting, alerting, and downstream computing and transferring features. For more information, see [Overview](/intl.en-US/Data Collection/Cloud product collection/Inner-ActionTrail/Usage notes.md).

## Monitoring service

The monitoring service of OSS provides metrics to measure the running status and performance of the system. The monitoring service also provides a custom alert service to help you track requests, analyze usage, collect statistics for business trends, and discover and diagnose system problems in a timely manner. For more information about the monitoring service, see [Monitoring service](/intl.en-US/Developer Guide/Monitoring service/Overview.md).

## SDDP

Data stored in OSS may include sensitive information such as personal data, passwords, keys, and sensitive images. You can combine OSS with Sensitive Data Discovery and Protection \(SDDP\) to better identify, classify, and protect sensitive data. After you authorize SDDP to scan your OSS buckets, SDDP identifies sensitive data in your OSS buckets, classifies and displays sensitive data by risk level, and tracks the use of sensitive data. In addition, SDDP protects and audits sensitive data based on built-in security rules, so that you can query the security status of your data assets in OSS buckets at any time. For more information, see [Sensitive data protection](/intl.en-US/Best Practices/Sensitive data protection.md).

