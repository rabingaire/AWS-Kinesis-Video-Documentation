# GetMediaForFragmentList<a name="API_reader_GetMediaForFragmentList"></a>

Gets media for a list of fragments \(specified by fragment number\) from the archived data in a Kinesis video stream\.

The following limits apply when using the `GetMediaForFragmentList` API:
+ A client can call `GetMediaForFragmentList` up to five times per second per stream\. 
+ Kinesis Video Streams sends media data at a rate of up to 25 megabytes per second \(or 200 megabits per second\) during a `GetMediaForFragmentList` session\. 

## Request Syntax<a name="API_reader_GetMediaForFragmentList_RequestSyntax"></a>

```
POST /getMediaForFragmentList HTTP/1.1
Content-type: application/json

{
   "[Fragments](#KinesisVideo-reader_GetMediaForFragmentList-request-Fragments)": [ "string" ],
   "[StreamName](#KinesisVideo-reader_GetMediaForFragmentList-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_GetMediaForFragmentList_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_GetMediaForFragmentList_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [Fragments](#API_reader_GetMediaForFragmentList_RequestSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-request-Fragments"></a>
A list of the numbers of fragments for which to retrieve media\. You retrieve these values with [ListFragments](API_reader_ListFragments.md)\.  
Type: Array of strings  
Array Members: Minimum number of 1 item\. Maximum number of 1000 items\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[0-9]+$`   
Required: Yes

 ** [StreamName](#API_reader_GetMediaForFragmentList_RequestSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-request-StreamName"></a>
The name of the stream from which to retrieve fragment media\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

## Response Syntax<a name="API_reader_GetMediaForFragmentList_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-Type: ContentType

Payload
```

## Response Elements<a name="API_reader_GetMediaForFragmentList_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The response returns the following HTTP headers\.

 ** [ContentType](#API_reader_GetMediaForFragmentList_ResponseSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-response-ContentType"></a>
The content type of the requested media\.  
Length Constraints: Minimum length of 1\. Maximum length of 128\.  
Pattern: `^[a-zA-Z0-9_\.\-]+$` 

The response returns the following as the HTTP body\.

 ** [Payload](#API_reader_GetMediaForFragmentList_ResponseSyntax) **   <a name="KinesisVideo-reader_GetMediaForFragmentList-response-Payload"></a>
The payload that Kinesis Video Streams returns is a sequence of chunks from the specified stream\. For information about the chunks, see [PutMedia](http://docs.aws.amazon.com/kinesisvideostreams/latest/dg/API_dataplane_PutMedia.html)\. The chunks that Kinesis Video Streams returns in the `GetMediaForFragmentList` call also include the following additional Matroska \(MKV\) tags:   
+ AWS\_KINESISVIDEO\_FRAGMENT\_NUMBER \- Fragment number returned in the chunk\.
+ AWS\_KINESISVIDEO\_SERVER\_SIDE\_TIMESTAMP \- Server\-side time stamp of the fragment\.
+ AWS\_KINESISVIDEO\_PRODUCER\_SIDE\_TIMESTAMP \- Producer\-side time stamp of the fragment\.
The following tags will be included if an exception occurs:  
+ AWS\_KINESISVIDEO\_FRAGMENT\_NUMBER \- The number of the fragment that threw the exception
+ AWS\_KINESISVIDEO\_EXCEPTION\_ERROR\_CODE \- The integer code of the exception
+ AWS\_KINESISVIDEO\_EXCEPTION\_MESSAGE \- A text description of the exception

## Errors<a name="API_reader_GetMediaForFragmentList_Errors"></a>

For information about the errors that are common to all actions, see [Common Errors](CommonErrors.md)\.

 **ClientLimitExceededException**   
Kinesis Video Streams has throttled the request because you have exceeded the limit of allowed client calls\. Try making the call later\.  
HTTP Status Code: 400

 **InvalidArgumentException**   
A specified parameter exceeds its restrictions, is not supported, or can't be used\.  
HTTP Status Code: 400

 **NotAuthorizedException**   
Status Code: 403, The caller is not authorized to perform an operation on the given stream, or the token has expired\.  
HTTP Status Code: 401

 **ResourceNotFoundException**   
Kinesis Video Streams can't find the stream that you specified\.  
HTTP Status Code: 404

## See Also<a name="API_reader_GetMediaForFragmentList_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/GetMediaForFragmentList) 