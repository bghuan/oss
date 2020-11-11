# Manage symbolic links

This topic describes how to create a symbolic link and obtain the name of the object to which the symbolic link points.

## Create a symbolic link

A symbolic link is a special object that maps to an object similar to a shortcut used in Windows. You can configure user-defined Object Meta for symbolic links.

The following code provides an example on how to create a symbolic link:

```
PutSymlinkRequest putSymlink = new PutSymlinkRequest();
putSymlink.setBucketName("yourBucketName");
// Set the name of the symbolic link.
putSymlink.setObjectKey("yourSymLink");
// Set the name of the object to which the symbolic link points.
putSymlink.setTargetObjectName("yourTargetObjectName");

OSSAsyncTask task = oss.asyncPutSymlink(putSymlink, new OSSCompletedCallback<PutSymlinkRequest,
        PutSymlinkResult>() {
    @Override
    public void onSuccess(PutSymlinkRequest request, PutSymlinkResult result) {
        OSSLog.logInfo("code:"+result.getStatusCode());

    }

    @Override
    public void onFailure(PutSymlinkRequest request, ClientException clientException,
                          ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());

    }
});
task.waitUntilFinished();
```

## Obtain the name of the object to which the symbolic link points

To obtain a symbolic link, you must have the read permissions on it. The following code provides an example on how to obtain the name of the object to which the symbolic link points:

```
// Obtain the information about the object to which the symbolic link points.
GetSymlinkRequest getSymlink = new GetSymlinkRequest();
getSymlink.setBucketName("yourBucketName");
getSymlink.setObjectKey("yourSymLink");

OSSAsyncTask task = oss.asyncGetSymlink(getSymlink, new OSSCompletedCallback<GetSymlinkRequest,
        GetSymlinkResult>() {
    @Override
    public void onSuccess(GetSymlinkRequest request, GetSymlinkResult result) {
        OSSLog.logInfo("targ::"+result.getTargetObjectName());

    }

    @Override
    public void onFailure(GetSymlinkRequest request, ClientException clientException,
                          ServiceException serviceException) {
        OSSLog.logError("error: "+serviceException.getRawMessage());

    }
});
task.waitUntilFinished();
```

