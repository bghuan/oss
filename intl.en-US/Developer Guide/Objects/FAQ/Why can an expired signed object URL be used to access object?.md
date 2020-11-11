# Why can an expired signed object URL be used to access object?

In most cases, you cannot access an object by using an expired signed object URL. However, if the URL is opened in your local browser, you can access the object by using the browser cache while the browser cache is valid.

You can modify the object metadata to specify that a signed object URL becomes invalid when it expires. You can set Cache-Control to `no-cache` or set Expires to a value smaller than the expiration time of the signed object URL. For more information, see [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage files/Manage object metadata.md).

**Note:** After you set the Cache-Control or Expires parameter, the browser stops caching OSS objects or the validity periods of caches decrease. Consequently, the request count and outbound traffic over Internet may increase.

