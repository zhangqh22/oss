# UploadPart

You can call this operation to initialize a MultipartUpload task and upload data by part based on the specified object name and upload ID.

## Usage notes

Note the following items when you call the UploadPart operation:

-   Before you call UploadPart to upload data by part, you must call the InitiateMultipartUpload operation to obtain an upload ID generated by OSS.
-   If you use the same partNumber to upload a new part, the existing part that is uploaded by the partNumber is overwritten.
-   OSS adds the MD5 hash of each received part as the ETag header in the response.
-   If the x-oss-server-side-encryption request header is specified when you call InitiateMultipartUpload, the uploaded part is encoded. The x-oss-server-side-encryption header is included in the response to the UploadPart request to indicate the server-side encryption method of the part. For more information, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md).

## Request syntax

```
PUT /ObjectName? partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
```

## Request elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|partNumber|Positive integer|Yes|The number that identifies a part.Valid values: 1 to 10000.

The size of a part ranges from 100 KB to 5 GB.

**Note:** In multipart upload, each part except the last part must be larger than or equal to 100 KB in size. The size of each part is not verified when you call UploadPart because not all parts are uploaded and OSS does not know which part is the last part. The size of each part is verified only when you call CompleteMultipartUpload. |
|uploadId|String|Yes|The ID that identifies the object to which a part belongs.|

## Examples

Sample request

```
PUT /multipart.data? partNumber=1&uploadId=0004B9895DBBB6EC98E36  HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length: 6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****:J/lICfXEvPmmSW86bBAfMmUm****
[6291456 bytes data]
```

Sample success response

```
HTTP/1.1 200 OK
Server: AliyunOSS
Connection: keep-alive
ETag: "7265F4D211B56873A381D321F586****"
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0****
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

## SDK

You can use SDKs for the following programming languages to call the UploadPart operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [Python](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [C++](/intl.en-US/SDK Reference/C++/Upload objects/Multipart upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)
-   [Android](/intl.en-US/SDK Reference/Android/Upload objects/Multipart upload.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/Upload objects/Multipart upload.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidArgument|400|The error message returned because the part number is not within the range of 1 to 10000.|
|InvalidDigest|400|The error message returned because the Content-MD5 value in the request is inconsistent with the MD5 hash calculated by OSS. To ensure that no errors occur during data transmission over the network, you can include the Content-MD5 value in the request. OSS calculates the MD5 hash of the uploaded data and compares it with the Content-MD5 value in the request.|
