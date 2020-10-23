# API概览

本文介绍对象存储OSS提供的相关API接口及其各API接口的用法。

## 关于Service操作

|API|描述|
|---|--|
|[GetService \(ListBuckets\)](/intl.zh-CN/API 参考/关于Service操作/GetService (ListBuckets).md)|返回请求者拥有的所有存储空间（Bucket）|

## 关于Bucket的操作

|API|描述|
|:--|:-|
|[PutBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/PutBucket.md)|创建Bucket|
|[DeleteBucket](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/DeleteBucket.md)|删除Bucket|
|[GetBucket\(ListObject\)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)|列出Bucket中所有文件（Object）的信息|
|[GetBucketInfo](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketInfo.md)|获取Bucket信息|
|[GetBucketLocation](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketLocation.md)|获得Bucket所属的位置信息|
|[PutBucketAcl](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/PutBucketACL.md)|设置Bucket访问权限|
|[GetBucketAcl](/intl.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/GetBucketAcl.md)|获得Bucket访问权限|
|[PutBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/PutBucketLifecycle.md)|设置Bucket中Object的生命周期规则|
|[GetBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/GetBucketLifecycle.md)|查看Bucket中Object的生命周期规则|
|[DeleteBucketLifecycle](/intl.zh-CN/API 参考/关于Bucket的操作/生命周期（Lifecycle）/DeleteBucketLifecycle.md)|删除Bucket中Object的生命周期规则|
|[PutBucketVersioning](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/PutBucketVersioning.md)|设置Bucket的版本控制状态|
|[GetBucketVersioning](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/GetBucketVersioning.md)|获取Bucket的版本控制状态|
|[GetBucketVersions\(ListObjectVersions\)](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/GetBucketVersions(ListObjectVersions).md)|列举Bucket中所有Object的版本信息|
|[PutBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/PutBucketReplication.md)|设置Bucket的跨区域复制规则|
|[GetBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplication.md)|查看Bucket已设置的跨区域复制规则|
|[GetBucketReplicationLocation](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplicationLocation.md)|查看可复制到的目标Bucket所在的地域|
|[GetBucketReplicationProgress](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/GetBucketReplicationProgress.md)|查看Bucket的跨区域复制进度|
|[DeleteBucketReplication](/intl.zh-CN/API 参考/关于Bucket的操作/跨区域复制（Replication）/DeleteBucketReplication.md)|停止Bucket的跨区域复制任务并删除Bucket的复制配置|
|[PutBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/PutBucketPolicy.md)|设置Bucket Policy|
|[GetBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/GetBucketPolicy.md)|获取Bucket Policy|
|[DeleteBucketPolicy](/intl.zh-CN/API 参考/关于Bucket的操作/授权策略（Policy）/DeleteBucketPolicy.md)|删除Bucket Policy|
|[PutBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/PutBucketInventory.md)|设置Bucket清单规则|
|[GetBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/GetBucketInventory.md)|查看Bucket中指定的清单任务|
|[ListBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/ListBucketInventory.md)|查看Bucket中所有的清单任务|
|[DeleteBucketInventory](/intl.zh-CN/API 参考/关于Bucket的操作/清单（Inventory）/DeleteBucketInventory.md)|删除Bucket中指定的清单任务|
|[InitiateBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/InitiateBucketWorm.md)|新建合规保留策略|
|[AbortBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/AbortBucketWorm.md)|删除未锁定的合规保留策略|
|[CompleteBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/CompleteBucketWorm.md)|锁定合规保留策略|
|[ExtendBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/ExtendBucketWorm.md)|延长已锁定的合规保留策略对应Bucket中Object的保留天数|
|[GetBucketWorm](/intl.zh-CN/API 参考/关于Bucket的操作/合规保留策略（WORM）/GetBucketWorm.md)|查看Bucket的合规保留策略信息|
|[PutBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/PutBucketLogging.md)|开启Bucket访问日志记录功能|
|[GetBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/GetBucketLogging.md)|查看Bucket的访问日志配置情况|
|[DeleteBucketLogging](/intl.zh-CN/API 参考/关于Bucket的操作/日志管理（Logging）/DeleteBucketLogging.md)|关闭Bucket访问日志记录功能|
|[PutBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/PutBucketWebsite.md)|设置Bucket为静态网站托管模式|
|[GetBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/GetBucketWebsite.md)|查看Bucket的静态网站托管状态|
|[DeleteBucketWebsite](/intl.zh-CN/API 参考/关于Bucket的操作/静态网站（Website）/DeleteBucketWebsite.md)|关闭Bucket的静态网站托管模式|
|[PutBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/PutBucketReferer.md)|设置Bucket的防盗链规则|
|[GetBucketReferer](/intl.zh-CN/API 参考/关于Bucket的操作/防盗链（Referer）/GetBucketReferer.md)|查看Bucket的防盗链规则|
|[PutBucketTags](/intl.zh-CN/API 参考/关于Bucket的操作/标签（Tags）/PutBucketTags.md)|添加或修改Bucket标签|
|[GetBucketTags](/intl.zh-CN/API 参考/关于Bucket的操作/标签（Tags）/GetBucketTags.md)|查看Bucket标签信息|
|[DeleteBucketTags](/intl.zh-CN/API 参考/关于Bucket的操作/标签（Tags）/DeleteBucketTags.md)|删除Bucket标签|
|[PutBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/PutBucketEncryption.md)|配置Bucket的加密规则|
|[GetBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/GetBucketEncryption.md)|获取Bucket的加密规则|
|[DeleteBucketEncryption](/intl.zh-CN/API 参考/关于Bucket的操作/加密（Encryption）/DeleteBucketEncryption.md)|删除Bucket的加密规则|
|[PutBucketRequestPayment](/intl.zh-CN/API 参考/关于Bucket的操作/请求者付费（RequestPayment）/PutBucketRequestPayment.md)|设置Bucket为请求者付费模式|
|[GetBucketRequestPayment](/intl.zh-CN/API 参考/关于Bucket的操作/请求者付费（RequestPayment）/GetBucketRequestPayment.md)|查看Bucket请求者付费模式配置信息|

## 关于Object的操作

|API|描述|
|:--|:-|
|[PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)|上传Object|
|[CopyObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/CopyObject.md)|拷贝Object|
|[GetObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObject.md)|获取Object|
|[AppendObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/AppendObject.md)|以追加写的方式上传Object|
|[DeleteObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteObject.md)|删除Object|
|[DeleteMultipleObjects](/intl.zh-CN/API 参考/关于Object操作/基础操作/DeleteMultipleObjects.md)|删除多个Object|
|[HeadObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/HeadObject.md)|只返回某个Object的meta信息，不返回文件内容|
|[GetObjectMeta](/intl.zh-CN/API 参考/关于Object操作/基础操作/GetObjectMeta.md)|返回Object的基本meta信息，包括该Object的ETag、Size（文件大小）以及LastModified等，不返回文件内容|
|[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)|通过HTML表单上传的方式上传Object|
|[PutObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/PutObjectACL.md)|修改Object的访问权限|
|[GetObjectACL](/intl.zh-CN/API 参考/关于Object操作/权限控制（ACL)/GetObjectACL.md)|查看Object的访问权限|
|[Callback](/intl.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)|上传回调|
|[PutSymlink](/intl.zh-CN/API 参考/关于Object操作/软链接（Symlink）/PutSymlink.md)|创建软链接|
|[GetSymlink](/intl.zh-CN/API 参考/关于Object操作/软链接（Symlink）/GetSymlink.md)|获取软链接|
|[RestoreObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/RestoreObject.md)|解冻文件|
|[SelectObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/SelectObject.md)|用SQL语法查询Object内容|
|[PutObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/PutObjectTagging.md)|设置或更新对象标签|
|[GetObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/GetObjectTagging.md)|获取对象标签信息|
|[DeleteObjectTagging](/intl.zh-CN/API 参考/关于Object操作/标签（Tagging）/DeleteObjectTagging.md)|删除指定的对象标签|

## 关于Multipart Upload的操作

|API|描述|
|:--|:-|
|[InitiateMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/InitiateMultipartUpload.md)|初始化MultipartUpload事件|
|[UploadPart](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/UploadPart.md)|分块上传文件|
|[UploadPartCopy](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/UploadPartCopy.md)|分块复制上传文件|
|[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)|完成整个文件的MultipartUpload上传|
|[AbortMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/AbortMultipartUpload.md)|取消MultipartUpload事件|
|[ListMultipartUploads](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/ListMultipartUploads.md)|列举所有执行中的MultipartUpload事件|
|[ListParts](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/ListParts.md)|列举指定UploadID所属的所有已上传成功的Part|

## 跨域资源共享（CORS）

|API|描述|
|:--|:-|
|[PutBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/PutBucketCors.md)|为Bucket设置CORS规则|
|[GetBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/GetBucketCors.md)|查看Bucket当前的CORS规则|
|[DeleteBucketCors](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/DeleteBucketCors.md)|关闭Bucket的CORS功能并清空所有规则|
|[OptionObject](/intl.zh-CN/API 参考/关于Bucket的操作/跨域资源共享（Cors）/OptionObject.md)|跨域访问preflight请求|

## 关于Live Channel的操作

|API|描述|
|---|--|
|[PutLiveChannelStatus](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannelStatus.md)|切换LiveChannel的状态|
|[PutLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md)|创建LiveChannel|
|[GetVodPlaylist](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetVodPlaylist.md)|获取播放列表|
|[PostVodPlaylist](/intl.zh-CN/API 参考/关于LiveChannel的操作/PostVodPlaylist.md)|生成播放列表|
|[Get LiveChannelStat](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelStat.md)|获取LiveChannel的推流状态信息|
|[GetLiveChannelInfo](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelInfo.md)|获取LiveChannel的配置信息|
|[GetLiveChannelHistory](/intl.zh-CN/API 参考/关于LiveChannel的操作/GetLiveChannelHistory.md)|获取LiveChannel的推流记录|
|[ListLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/ListLiveChannel.md)|列举LiveChannel|
|[DeleteLiveChannel](/intl.zh-CN/API 参考/关于LiveChannel的操作/DeleteLiveChannel.md)|删除LiveChannel|

