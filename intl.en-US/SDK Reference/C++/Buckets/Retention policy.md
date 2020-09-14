# Retention policy

You can configure a time-based retention policy for a bucket. A retention policy has a protection period ranging from one day to 70 years. This topic describes how to create, query, and lock a retention policy.

## Create a retention policy

The following code provides an example on how to create a retention policy:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the Alibaba Cloud account information.*/
      std::string AccessKeyId = "yourAccessKeyId";
      std::string AccessKeySecret = "yourAccessKeySecret";
      std::string Endpoint = "yourEndpoint";
      std::string BucketName = "yourBucketName";

      /* Initialize network resources.*/
      InitializeSdk();

      ClientConfiguration conf;
      OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
        /* Create the retention policy and set the protection period to 10 days.*/
      auto outcome = client.InitiateBucketWorm(InitiateBucketWormRequest(BucketName, 10));

      if (outcome.isSuccess()) {      
            std::cout << " InitiateBucketWorm success " << std::endl;
      }
      else {
        /* Handle exceptions.*/
        std::cout << "InitiateBucketWorm fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
      }

      /* Release network resources.*/
      ShutdownSdk();
      return 0;
}
```

For more information about how to create a retention policy, see [InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md).

## Cancel an unlocked retention policy

The following code provides an example on how to cancel an unlocked retention policy:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the Alibaba Cloud account information.*/
      std::string AccessKeyId = "yourAccessKeyId";
      std::string AccessKeySecret = "yourAccessKeySecret";
      std::string Endpoint = "yourEndpoint";
      std::string BucketName = "yourBucketName";

      /* Initialize network resources.*/
      InitializeSdk();

      ClientConfiguration conf;
      OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
      /* Cancel the unlocked retention policy.*/
      auto outcome = client.AbortBucketWorm(AbortBucketWormRequest(BucketName));

      if (outcome.isSuccess()) {      
        std::cout << " AbortBucketWorm success " << std::endl;
      }
      else {
        /* Handle exceptions.*/
        std::cout << "AbortBucketWorm fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
      }

      /* Release network resources.*/
      ShutdownSdk();
      return 0;
}
```

For more information about how to cancel an unlocked retention policy, see [AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md).

## Lock a retention policy

The following code provides an example on how to lock a retention policy:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the Alibaba Cloud account information.*/
      std::string AccessKeyId = "yourAccessKeyId";
      std::string AccessKeySecret = "yourAccessKeySecret";
      std::string Endpoint = "yourEndpoint";
      std::string BucketName = "yourBucketName";
      std::string WormId = "yourWormId";

      /* Initialize network resources.*/
      InitializeSdk();

      ClientConfiguration conf;
      OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
        /* Lock the retention policy.*/
      auto outcome = client.CompleteBucketWorm(CompleteBucketWormRequest(BucketName, WormId));

      if (outcome.isSuccess()) {      
        std::cout << " CompleteBucketWorm success " << std::endl;
      }
      else {
        /* Handle exceptions.*/
        std::cout << "CompleteBucketWorm fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
      }

      /* Release network resources.*/
      ShutdownSdk();
      return 0;
}
```

For more information about how to lock a retention policy, see [CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md).

## Query a retention policy

The following code provides an example on how to query a retention policy:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the Alibaba Cloud account information.*/
      std::string AccessKeyId = "yourAccessKeyId";
      std::string AccessKeySecret = "yourAccessKeySecret";
      std::string Endpoint = "yourEndpoint";     
      std::string BucketName = "yourBucketName";

      /* Initialize network resources.*/
      InitializeSdk();

      ClientConfiguration conf;
      OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
        /* Query the retention policy.*/
      auto outcome = client.GetBucketWorm(GetBucketWormRequest(BucketName));

      if (outcome.isSuccess()) {      
            std::cout << " GetBucketWorm success " << std::endl;
            std::cout << " CreationDate:" << outcome.result().CreationDate() <<
            ",State:" << outcome.result().State() <<
            ",WormId:" << outcome.result().WormId() << std::endl;
      }
      else {
        /* Handle exceptions.*/
        std::cout << "GetBucketWorm fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
      }

      /* Release network resources.*/
      ShutdownSdk();
      return 0;
}
```

For more information about how to query a retention policy, see [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md).

## Extend the retention period of a retention policy

The following code provides an example on how to extend the retention period of a locked retention policy:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
      /* Initialize the Alibaba Cloud account information.*/
      std::string AccessKeyId = "yourAccessKeyId";
      std::string AccessKeySecret = "yourAccessKeySecret";
      std::string Endpoint = "yourEndpoint";
      std::string BucketName = "yourBucketName";
      std::string WormId = "yourWormId";

      /* Initialize network resources.*/
      InitializeSdk();

      ClientConfiguration conf;
      OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
  
        /* Extend the retention period of the locked retention policy.*/
      auto outcome = client.ExtendBucketWormWorm(ExtendBucketWormRequest(BucketName, WormId, 20));

      if (outcome.isSuccess()) {      
        std::cout << " ExtendBucketWormWorm success " << std::endl;
      }
      else {
        /* Handle exceptions.*/
        std::cout << "ExtendBucketWormWorm fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
      }

      /* Release network resources.*/
      ShutdownSdk();
      return 0;
}
```

For more information about how to extend the retention period of a locked retention policy, see [ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md).

