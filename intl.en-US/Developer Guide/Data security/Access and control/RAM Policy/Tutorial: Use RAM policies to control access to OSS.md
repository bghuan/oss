# Tutorial: Use RAM policies to control access to OSS

This tutorial demonstrates how to use RAM policies to control access to OSS buckets, folders and objects.

## Prerequisites

Before you use RAM policies to control access to OSS, ensure the following conditions are met:

-   A bucket is created.

    A bucket named example-company is created by an Alibaba Cloud account in this topic as an example. For more information about how to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

-   Objects and folders are uploaded.

    In this topic, an object named oss-dg.pfg and the following three folders are uploaded to the root directory of the example-company bucket: Development/, Marketing/, and Private/. The following table describes the objects contained in each folder and their names with prefixes.

    |Folder|Object with prefix|
    |------|------------------|
    |Development/|Development/Alibaba Cloud.pdf, Development/ProjectA.docx, and Development/ProjectB.docx|
    |Marketing/|Marketing/data2016.xlsx and Marketing/example2016.txt|
    |Private/|Private/2017/images.zip and Private/2017/promote.pptx|

    For more information about how to upload objects, see [Upload objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload objects.md).

-   RAM users are created.

    Two RAM users named Anne and Leo are created by an Alibaba Cloud account in this topic as an example. For more information about how to create RAM users, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).


## Background information

RAM policies are configured based on users. You can manage users by configuring RAM policies. For users such as employees, systems, or applications, you can control which resources are accessible. For example, you can create a RAM policy to grant users read permissions on a bucket.

RAM policies are in the JSON format. A RAM policy includes the following fields:

-   Statement: the authorization statement. A RAM policy can include multiple authorization statements.
-   Effect: the effect of the policy . Valid values: Allow and Deny.

    **Note:** If a RAM policy includes an Allow statement and a Deny statement, the Deny statement takes precedence over the Allow statement.

-   Action: the authorized actions on resources.

If you use RAM policies, we recommend that you use RAM Policy Editor to generate required RAM policies. For more information, see [RAM Policy Editor](/intl.en-US/Tools/RAM Policy Editor.md).

Compared with RAM policies, bucket policies can be configured in the OSS console. The bucket owner can grant other users permissions to access OSS resources. For more information, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use bucket policies to authorize other users to access OSS resources.md).

## Scenario and implementation

In this example, you need to grant the RAM users Anne and Leo permissions to access resources in different folders as follows:

-   Grant Anne permissions to access objects and subfolders only in Development/. For more information, see [Step 2: Grant RAM user Anne specific permissions](#section_gqp_p63_kl8).
-   Grant Leo permissions to access objects and subfolders only in Marketing/. For more information, see [Step 3: Grant RAM user Leo specific permissions](#section_fvm_dph_z1k).
-   Set the ACL of the Private/ folder to private, which indicates that all RAM users created by the Alibaba Cloud account cannot access the folder.

Based on the scenario in this topic, the following permissions must be granted to both Anne and Leo:

-   Permissions to list all buckets owned by the Alibaba Cloud account, that is, permissions to perform oss:ListBuckets.
-   Permissions to list objects and folders in the root directory of the example-company bucket, that is, permissions to perform oss:ListObjects.

In this case, you can create RAM user groups to classify and authorize RAM users created by the Alibaba Cloud account for easier user and permission management. For more information about how to create and authorize user groups, see [Step 1: Create a RAM user group and grant permissions to the group](#section_tu8_3br_l9p).

## Before you begin

Before you grant permissions to RAM users, you must understand how the OSS console interacts with OSS when you click a bucket name in the console.

-   Send a request to access a bucket

    When you click the example-company bucket in the console, the console sends a [GetBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md) request to OSS.

    -   Sample request

        ```
        GET /? prefix=&delimiter=/ HTTP/1.1
        Host: example-company.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        In the preceding request, the value of the prefix parameter is an empty string and the value of the delimiter parameter is a forward slash \(/\).

    -   Sample response

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <? xml version="1.0" encoding="UTF-8"? >
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>example-company</Name>
        <Prefix></Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>oss-dg.pdf</Key>
                ...
            </Contents>
           <CommonPrefixes>
                <Prefix>Development</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Marketing</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Private</Prefix>
           </CommonPrefixes>
        </ListBucketResult>
        ```

    -   Response resolution

        The console resolves the response returned by OSS and displays the following items in the root directory of the example-company bucket.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0595688951/p1189.png)

-   Send a request to access a folder in the bucket

    When you click the Development/ folder in the console, the console sends a [GetBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md) request to OSS. The request includes the prefix and delimiter parameters.

    -   Sample request

        ```
        GET /? prefix=Development/&delimiter=/ HTTP/1.1
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        In the request, the value of the prefix parameter is `Development/` and the value of the delimiter parameter is a forward slash \(/\).

    -   Sample response

        In the response, OSS returns object keys with the specified prefix.

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <? xml version="1.0" encoding="UTF-8"? >
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>example-company</Name>
        <Prefix>Development/</Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>ProjectA.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>ProjectB.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>Alibaba Cloud.pdf</Key>
                ...
            </Contents>
        </ListBucketResult>
        ```

    -   Response resolution

        The console resolves the response returned by OSS and displays the following objects in the Development/ folder.

        ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/0595688951/p1190.png)


## Step 1: Create a RAM user group and grant permissions to the group

In this topic, a user group named Staff is created as an example. For more information about how to create a user group, see [Create a RAM user group](/intl.en-US/RAM User Group Management/Create a RAM user group.md).

-   Grant RAM users permissions to list all buckets owned by the Alibaba Cloud account
    1.  Create a RAM policy.

        In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                  "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
           "Resource": [
             "acs:oss:*:*:*"
           ]
         }
        ]
        }
        ```

        For more information about how to create a RAM policy, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

    2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to the Staff group.

        For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

-   Grant RAM users permissions to list items in the root directory of the bucket

    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Permissions** \> **Policies**.
    3.  Find the created `AllowGroupToSeeBucketListInConsole` policy and click the policy name.
    4.  On the page that appears, click the **Policy Document** tab.

        To list items in the bucket, the RAM user must have permissions to perform the `oss:ListObjects` operation.

        ```
        {
           "Version": "1",
           "Statement": [
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                 "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
               "Resource": [
                 "acs:oss:*:*:*"
               ],
               "Condition": {}
             },
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListObjects"
               ],
               "Resource": [
                 "acs:oss:*:*:example-company"
               ],
               "Condition": {
                 "StringLike": {
                   "oss:Prefix": [
                     ""
                   ],
                   "oss:Delimiter": [
                     "/"
                   ]
                 }
               }
             }
           ]
         }
        ```

    **Note:** You can modify a RAM policy for a maximum of five times. To modify a RAM policy that has been modified for five times, you must delete the policy, create a new one, and assign the new policy to the Staff group.


## Step 2: Grant RAM user Anne specific permissions

You can perform the following steps to grant RAM user Anne permissions to access all objects in the Development/ folder.

1.  In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Development/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",         
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Development/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to RAM user Anne.


To allow RAM user Anne to not only access objects in the Development/ folder but also write objects to the folder, create the following RAM policy:

```
{
   "Version": "1",
   "Statement": [
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:example-company"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Development/*"
           ]
         }
       }
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:GetObject",
         "oss:PutObject",
         "oss:GetObjectAcl"
       ],
       "Resource": [
         "acs:oss:*:*:example-company/Development/*"
       ],
       "Condition": {}
     }
   ]
 }
```

## Step 3: Grant RAM user Leo specific permissions

You can perform the following steps to grant RAM user Leo permissions to access all objects in the Marketing/ folder.

1.  In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Marketing/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",         
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Marketing/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to RAM user Leo.


## Step 4: Prohibit RAM users from accessing the Private folder

In this topic, you need to set the ACL of the Private/ folder to private, which indicates that all RAM users created by the Alibaba Cloud account cannot access the folder. Therefore, you must add a policy that explicitly denies access to the Private folder. This policy takes precedence over other RAM users. You can perform the following steps to add the policy.

-   Add the following statement to explicitly deny any access to the Private folder \(example-company/Private/\*\):

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:*"
        ],
        "Resource": [
          "acs:oss:*:*:example-company/Private/*"
        ],
        "Condition": {}
      }
    ```

-   Deny the `ListObjects` operations when a request is sent to access objects with the Private prefix.

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:ListObjects"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ],
        "Condition": {
          "StringEquals": {
            "oss:Prefix": [
              "Private/"
            ]
          }
        }
      }
    ```

    After the preceding policy is added, OSS returns an error when the RAM users to list the Private/2017/images.zip and Private/2017/promote.pptx objects in the Private folder.

-   Replace the policy `AllowGroupToSeeBucketListInConsole` for the Staff group with an updated policy that contains the preceding deny statements.

    After the updated policy is applied, none of the RAM users in the user group can access the Private folder in your bucket.

    ```
    {
    "Version": "1",
    "Statement": [
     {
       "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketStat",
             "oss:GetBucketInfo",
             "oss:GetBucketAcl"
           ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:example-company"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
              ""
            ],
           "oss:Delimiter": [
             "/"
           ]
         }
       }
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:*"
       ],
       "Resource": [
         "acs:oss:*:*:example-company/Private/*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Private/"
           ]
         }
       }
     }
    ]
    }
    ```


