# Event notification

OSS allows you to configure event notification to learn about related operations on resources in a timely manner.

When you create an event notification rule, you can customize the event notification rule. This way, you can be notified of specified object operations in OSS in a timely manner. Examples:

-   New data is uploaded from image sharing platforms or audio and video platforms to OSS.
-   Relevant content in OSS is updated.
-   Important objects in OSS are deleted.
-   Data synchronization in OSS is completed.

**Note:**

-   Notifications are not sent for the TS and M3U8 objects that are generated by using RTMP ingest.
-   The event notification feature is available in all regions except the China \(Heyuan\), China \(Hohhot\), UAE \(Dubai\) and Malaysia \(Kuala Lumpur\) regions.

Event notification in OSS is asynchronous and does not affect normal OSS operations. To configure event notification, you must configure the rule and message notification.

-   Rule: specifies conditions for OSS resource usage for which to send notifications.
-   Message notification: relies on Alibaba Cloud MNS to provide multiple notification methods.

The following figure shows the overall architecture of event notification for OSS resources.

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8795688951/p63749.png)

## Implementation mode

For more information about the configurations of event notification, see [Configure event notification](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure event notification.md).

## Event types

|Event type|Description|
|----------|-----------|
|ObjectCreated:PutObject|Created or overwritten an object: simple upload.|
|ObjectCreated:PostObject|Created or overwritten object: form upload.|
|ObjectCreated:CopyObject|Created or overwritten an object: object copy.|
|ObjectCreated:InitiateMultipartUpload|Created or overwritten an object: A multipart upload task was initialized.|
|ObjectCreated:UploadPart|Created or overwritten an object: Parts were uploaded.|
|ObjectCreated:UploadPartCopy|Created or overwritten an object: Parts were uploaded by copying data from an existing object.|
|ObjectCreated:CompleteMultipartUpload|Created or overwritten an object: A multipart upload task was completed.|
|ObjectCreated:AppendObject|Created or overwritten an object: append upload.|
|ObjectDownloaded:GetObject|Downloaded an object: simple download.|
|ObjectRemoved:DeleteObject|An object was deleted.|
|ObjectRemoved:DeleteObjects|Multiple objects were deleted.|
|ObjectReplication:ObjectCreated|Synchronized an operation on an object: The object was created.|
|ObjectReplication:ObjectRemoved|Synchronized an operation on an object: The object was deleted.|
|ObjectReplication:ObjectModified|Synchronized an operation on an object: The object was overwritten.|
|ObjectCreated:All|Created or overwritten objects: Any object was created or overwritten.|
|ObjectDownloaded:All|Downloaded objects: Any object was downloaded.|
|ObjectRemoved:All|Any object was deleted.|

**Note:** Three types of event `ObjectCreated:All`, `ObjectDownloaded:All`, `ObjectRemoved:All` are only available in China \(Hong Kong\), US \(Silicon Valley\), US \(Virginia\), Germany \(Frankfurt\), Australia \(Sydney\), Singapore, and UK \(London\) regions. These event types will be available in other regions soon.

## Notification message formats

The message content of OSS-based event notification is encoded in Base64. The decoded content is in the JSON format. Example:

```
{"events": [{
    "eventName": "",  // The event type.
    "eventSource": "", // The resource on which operations were performed and triggered event notification. The value is acs:oss.
    "eventTime": "", // The time when the event was triggered. The returned data is in the ISO 8601 standard in the yyyy-MM-ddTHH:mm:ssZ format.
    "eventVersion": "", // The version number. The version number is 1.0.
    "oss": {
        "bucket": {
            "arn": "", // The ARN for the bucket. The format is "acs:oss:region:uid:bucket".
            "name": "", // The name of the bucket.
            "ownerIdentity": ""}, // The owner of the bucket.
        "object": {
            "deltaSize": , // The changed size of the object. If you create an object, this value is the size of the object. If you overwrite an existing object, this value is the difference between the new and existing objects, which may be a negative value.
            "eTag": "", // The ETag of the object. This value is the same as the ETag header value in the response to the GetObject() request.
            "key": "", // The name of the object.
            "position":, // Required only for the ObjectCreated:AppendObject event. This parameter indicates the position calculated based on the size of the object last uploaded. The value starts from 0.
            "readFrom": , // Required only for the ObjectDownloaded:GetObject event. This parameter indicates the beginning of the range of Bytes to read data from the object. The value starts from 0 for the range request. Otherwise, the value is 0.
            "readTo": , // Required only for the ObjectDownloaded:GetObject event. This parameter indicates the ending of the range of bytes to read data from the object. The value increases by one for the ending byte in the range request. Otherwise, the value is the actual size of the object.
            "size": }, // The size of the object.
        "ossSchemaVersion": "", // The version of the schema. The value is 1.0.
        "ruleId": "GetObject", // The rule ID that matches the event.
        "region": "", // The region in which the bucket is located.
        "requestParameters": {
            "sourceIPAddress": ""}, // The source IP address in the request.
        "responseElements": {
            "requestId": ""}, // The ID of the request.
        "userIdentity": {
            "principalId": ""}, // The user ID of the requester.
        "xVars": {  // The custom parameters in the callback of OSS.
            "x:callback-var1":"value1",
            "x:vallback-var2":"value2"}}}]}
```

Examples:

```
{"events": [{
    "eventName": "ObjectDownloaded:GetObject",
    "eventSource": "acs:oss",
    "eventTime": "2016-07-01T11:17:30.000Z",
    "eventVersion": "1.0",
    "oss": {
        "bucket": {
            "arn": "acs:oss:cn-shenzhen:11489********46818:event-notification-test-shenzhen",
            "name": "event-notification-test-shenzhen",
            "ownerIdentity": "11489********46818"},
        "object": {
            "deltaSize": 0,
            "eTag": "0CC175B9C0F1B6xxxxxx99E269772661",
            "key": "test",
            "readFrom": 0,
            "readTo": 1,
            "size": 1},
        "ossSchemaVersion": "1.0",
        "ruleId": "GetObjectRule",
        "region": "cn-shenzhen",
        "requestParameters": {
            "sourceIPAddress": "140.xx.xx.90"},
        "responseElements": {
            "requestId": "5776514Axxxxxxx542425D2B"},
        "userIdentity": {
            "principalId": "11489********46818"},
        "xVars": {
            "x:callback-var1":"value1",
            "x:vallback-var2":"value2"}}}]}
```
