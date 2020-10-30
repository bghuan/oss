# Traffic fees

Traffic is the cumulative value of the data flow generated when you access OSS, including outbound traffic over the Internet, outbound traffic over the internal network, inbound traffic over the Internet, inbound traffic over the internal network, CDN back-to-origin outbound traffic, and cross-region replication \(CRR\) traffic. OSS charges total usage of traffic generated when you access OSS.

**Note:** This topic describes the definition and billing methods of billing items for traffic usage. For more information about pricing, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing).[Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing).

|Billing item|Description|Billing method|
|------------|-----------|--------------|
|Outbound traffic over the Internet|Traffic generated when data is transferred from OSS to the client over the Internet.|OSS provides the free quota of 5 GB for outbound traffic over the Internet. The traffic that exceeds 5 GB is charged based on the unit price of the tier.

-   Pay-as-you-go:

    -   Traffic usage ≤ 5 GB: free of charge.
    -   Traffic usage \> 5 GB: Fees = Unit price of the current tier × Outbound traffic over the Internet.

Example: A total of 100 GB of outbound traffic over the Internet is generated when you use a bucket located in the US \(Virginia\) region. The fees for outbound traffic over the Internet this month: USD 0.076 × \(100 - 5\) GB = USD 7.22.

**Note:** Traffic in mainland China is generated during peak and nonpeak hours. For more information about prices, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing).

-   Subscription: downstream data transfer plan. |
|Inbound traffic over the Internet|Traffic generated when data is transferred from the client to OSS over the Internet.|Free of charge.|
|Outbound traffic over the internal network|Traffic generated when data is transferred from OSS to the client over the internal network. For more information about how to use internal endpoints, see [Use an internal endpoint to access OSS](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).|Free of charge.|
|Inbound traffic over the internal network|Traffic generated when data is transferred from the client to OSS over the internal network.|Free of charge.|
|CDN back-to-origin outbound traffic|Back-to-origin traffic generated when data is transferred from OSS to CDN edge nodes.|-   Pay-as-you-go: Traffic fees = Total CDN back-to-origin outbound traffic/hour \(GB\) × Unit price/GB.
-   Subscription: back-to-origin traffic plan. |
|CRR traffic|Outbound traffic generated when you use CRR to synchronize data from a source bucket to a destination bucket.|-   Pay-as-you-go: Traffic fees = Total CRR traffic/hour \(GB\) × Unit price/GB.
-   Subscription: none. |

