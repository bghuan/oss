# Manage symbolic links

This topic describes how to create a symbolic link and obtain the name of the object to which the symbolic link points.

## Create a symbolic link

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows. You can configure user-defined Object Meta for symbolic links.

Run the following code to create a symbolic link:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
const char *link_object_name = "<yourLinkObjectName>";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize the aos_string_t type. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether the CNAME is used. 0 indicates that the CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters, such as timeout periods. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Use the aos_http_io_initialize method in main() to initialize global resources such as the network and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory resources. aos_pool_t *pool is equivalent to apr_pool_t. The implementation code is included in the APR library. */
    aos_pool_t *pool;
    /* Create a new memory pool. The second parameter is NULL. This indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate the memory resources in the memory pool to the options. */
    oss_client_options = oss_request_options_create(pool);
    /* Use oss_client_options to initialize the client options. */
    init_options(oss_client_options);
    /* Initialize the parameters. */
    aos_string_t bucket;
    aos_string_t object;
    aos_string_t sym_object;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_set(&sym_object, link_object_name);
    resp_status = oss_put_symlink(oss_client_options, &bucket, &sym_object, &object, &resp_headers);
    if (aos_status_is_ok(resp_status)) {
        printf("put symlink succeeded\n");
    } else {
        printf("put symlink failed\n");      
    }
    /* Release the memory pool. This releases the memory resources that are allocated during the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

## Obtain the name of the object to which the symbolic link points

To obtain a symbolic link, you must have the read permissions on it. The following code provides an example on how to obtain the name of the object to which the symbolic link points:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *link_object_name = "<yourLinkObjectName>";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize the aos_string_t type. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether the CNAME is used. 0 indicates that the CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters, such as timeout periods. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Use the aos_http_io_initialize method in main() to initialize global resources such as the network and memory. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Create a memory pool to manage memory resources. aos_pool_t *pool is equivalent to apr_pool_t. The implementation code is included in the APR library. */
    aos_pool_t *pool;
    /* Create a new memory pool. The second parameter is NULL. This indicates that the pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information such as endpoint, access_key_id, access_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate the memory resources in the memory pool to the options. */
    oss_client_options = oss_request_options_create(pool);
    /* Use oss_client_options to initialize the client options. */
    init_options(oss_client_options);
    /* Initialize the parameters. */
    aos_string_t bucket;
    aos_string_t link_object;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    char *target_object_name = NULL;
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&link_object, link_object_name);
    resp_status = oss_get_symlink(oss_client_options, &bucket, &link_object, &resp_headers);
    if (aos_status_is_ok(resp_status)) {
        printf("get symlink succeeded\n");
        target_object_name = (char*)(apr_table_get(resp_headers, OSS_CANNONICALIZED_HEADER_SYMLINK));
        printf("target_object_name: %s \n", target_object_name);
    } else {
        printf("get symlink failed\n");      
    }
    /* Release the memory pool. This releases the memory resources that are allocated during the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

