# Use ECS instances that run Windows to configure reverse proxy for access to Alibaba Cloud OSS

IP addresses used to access a bucket dynamically change. You can use ECS instances to configure reverse proxy for access to OSS. This way, a static IP address can be used to access the bucket.

OSS allows you to use OSS domain names or custom domain names to access OSS. In some scenarios, you must use a static IP address to access OSS.

-   For security reasons, enterprises must configure outbound rules to specify that internal employees and business systems can access only the specified public IP address. However, the IP addresses used to access a bucket in OSS dynamically change. Enterprises must frequently modify firewall rules.
-   Due to the architecture limits of Alibaba Fiance Cloud, internal network-specific buckets in Alibaba Finance Cloud can be accessed only within Alibaba Finance Cloud.

To resolve these problems, you can use ECS instances to configure reverse proxy for access to OSS.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9454449951/p38572.png)

## Procedure

1.  Create an ECS instance that runs Windows Server and log on to the instance. Ensure that the instance and the specified bucket are located within the same region.

    The Windows Server 2019 Datacenter edition 64-bit system is used in this example. For more information, see [Quick start for Windows instances](/intl.en-US/Quick Start/Create an instance in the ECS console (detailed version)/Quick start for Windows instances.md).

2.  Download NGINX and decompress the package.

    NGINX-1.19.2 is used in this example. For more information about downloading URLs, visit [Nginx](http://nginx.org/en/download.html),

3.  Modify the configuration file.

    1.  Go to the conf folder and open the nginx.conf file by using a notepad.

    2.  Modify the content of the configuration file.

        ```
        worker_processes  1;
        events {
            worker_connections  1024;
        }
        
        http {
            include       mime.types;
            default_type  application/octet-stream;
        
            keepalive_timeout  65;
            server {
                listen       80;
                server_name  47. **. **.43;
        
                error_page   500 502 503 504  /50x.html;
                location = /50x.html {
                    root   html;
                }
        
               location / {
                    proxy_pass   http://bucketname.oss-cn-hangzhou-internal.aliyuncs.com;
                    #proxy_set_header Host $host;
                }
           }
        }
        ```

        -   server\_name: the IP address used to provide the reverse proxy service, which is the public IP address of the ECS instance.
        -   proxy\_pass: the endpoint for redirection.
            -   When the ECS instance and the requested bucket are located within the same region, specify the internal endpoint of the bucket. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).
            -   When the ECS instance and the requested bucket are located within different regions, specify the public endpoint of the bucket.
            -   For security reasons, when an OSS domain name is used to access an OSS image or web page object by using a browser, the object is directly downloaded. To preview the object, use a custom domain name that is bound to the bucket. Add the custom domain name for this parameter. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
        -   proxy\_set\_header Host $host: If you add this parameter, the host value is replaced with the IP address of the ECS instance when NGINX sends a request to OSS. You must add this parameter in the following situations:
            -   Signature errors.
            -   Your custom domain name is resolved to the public IP address of the ECS instance, and your user must browse image or web page objects in a bucket. You can bind a custom domain name to the bucket for which reverse proxy is configured. You do not need to configure CNAME. You can set proxy\_pass to the internal or public endpoint of the bucket. For more information about how to bind a custom domain name, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).
        **Note:**

        -   This topic uses the demo environment. For data security reasons, we recommend that you configure the https context.
        -   You can configure reverse proxy for only one bucket if you use this configuration method.
4.  Go to the folder of the NGINX executable. Double-click the nginx.exe and start NGINX.

5.  Open TCP port 80 of the ECS instance.

    By default, NGINX uses TCP port 80. Therefore, you must enable TCP port 80 when you configure a security group. For more information, see [Add security group rules](/intl.en-US/Security/Security groups/Add security group rules.md).

6.  Add the object path to the public IP address of the ECS instance to access OSS resources.

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/9454449951/p38588.png)


## References

-   [Use ECS instances that run Ubuntu to configure reverse proxy for access to Alibaba Cloud OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use ECS instances that run Ubuntu to configure reverse proxy for access to Alibaba Cloud OSS.md)
-   [Use ECS instances that run CentOS to configure reverse proxy for access to Alibaba Cloud OSS](/intl.en-US/Best Practices/Use ECS instances to configure reverse proxy for access to OSS/Use ECS instances that run CentOS to configure reverse proxy for access to Alibaba Cloud OSS.md)

