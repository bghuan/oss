# Tutorial: Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy

By default, access to OSS resources is restricted to the owner. To authorize another user to access your OSS resources, you can grant permissions for the user to access your bucket by adding a bucket policy.

Example: Company A wants to authorize Company B to access their OSS resources. However, Company A does not want to provide Company B with a RAM user. In this case, Company A can allow Company B to access their bucket by adding a bucket policy. After Company A adds a bucket policy that authorizes Company B to access their bucket, Company B can access the OSS bucket owned by Company A by adding the path of the bucket in the OSS console.

## Add the bucket policy

-   Use the Alibaba Cloud account of Company B to perform the following steps:
    1.  Log on to the [RAM console](https://ram.console.aliyun.com) to create a RAM user \(herein referred to as RAM User B\).

        For more information about how to add a resource group, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).

    2.  In the left-side navigation pane, click **Users**.
    3.  Click the username of the created RAM user to view and record the UID of the RAM user.
-   Use the Alibaba Cloud account of Company A to perform the following steps:
    1.  Log on to the [OSS console](https://oss.console.aliyun.com).
    2.  Click **Buckets**, and then click the name of the target bucket.
    3.  Choose **Files** \> **Authorize** \> **Authorize**.

        **Note:** You can also choose **Access Control** \> **Bucket Policy**. In the Bucket Policy section, click **Configure**. The **Authorize** dialog box appears.

    4.  In the **Authorize** dialog box that appears, configure the required parameters. Set **Accounts** to **Other Accounts**, and enter the UID of RAM User B. For more information about other parameters, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use bucket policies to authorize other users to access OSS resources.md).
    5.  Click **OK**.

## Log on to the OSS console as RAM User B and add the access path

After the bucket policy is added, you must log on to the OSS console as RAM User B and add the access path of the bucket of Company A. To add the access path, perform the following steps:

1.  Log on to the OSS console as RAM User B through the [RAM User logon link](http://signin.aliyun.com).

2.  Go to the [OSS console](https://oss.console.aliyun.com).

3.  In the left-side navigation pane, click the + icon next to **My OSS Paths**. In the Add Authorized OSS Path dialog box that appears, add the access path of the authorized bucket.

    -   **Region**: Select the region of the bucket of Company A from the drop-down list.
    -   **Bucket**: Enter the name of the bucket of Company A.
    -   **File Path**: Enter the path that Company A authorizes Company B to access. Example: To allow Company B to access only the test folder in the abc bucket, enter abc/test for File Path.

