# ListFragments<a name="API_reader_ListFragments"></a>

Returns a list of [Fragment](API_reader_Fragment.md) objects from the specified stream and start location within the archived data\.

## Request Syntax<a name="API_reader_ListFragments_RequestSyntax"></a>

```
POST /listFragments HTTP/1.1
Content-type: application/json

{
   "[FragmentSelector](#KinesisVideo-reader_ListFragments-request-FragmentSelector)": { 
      "[FragmentSelectorType](API_reader_FragmentSelector.md#KinesisVideo-Type-reader_FragmentSelector-FragmentSelectorType)": "string",
      "[TimestampRange](API_reader_FragmentSelector.md#KinesisVideo-Type-reader_FragmentSelector-TimestampRange)": { 
         "[EndTimestamp](API_reader_TimestampRange.md#KinesisVideo-Type-reader_TimestampRange-EndTimestamp)": number,
         "[StartTimestamp](API_reader_TimestampRange.md#KinesisVideo-Type-reader_TimestampRange-StartTimestamp)": number
      }
   },
   "[MaxResults](#KinesisVideo-reader_ListFragments-request-MaxResults)": number,
   "[NextToken](#KinesisVideo-reader_ListFragments-request-NextToken)": "string",
   "[StreamName](#KinesisVideo-reader_ListFragments-request-StreamName)": "string"
}
```

## URI Request Parameters<a name="API_reader_ListFragments_RequestParameters"></a>

The request does not use any URI parameters\.

## Request Body<a name="API_reader_ListFragments_RequestBody"></a>

The request accepts the following data in JSON format\.

 ** [FragmentSelector](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-FragmentSelector"></a>
Describes the time stamp range and time stamp origin for the range of fragments to return\.  
Type: [FragmentSelector](API_reader_FragmentSelector.md) object  
Required: No

 ** [MaxResults](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-MaxResults"></a>
The total number of fragments to return\. If the total number of fragments available is more than the value specified in `max-results`, then a [ListFragments:NextToken](#KinesisVideo-reader_ListFragments-response-NextToken) is provided in the output that you can use to resume pagination\.  
Type: Long  
Valid Range: Minimum value of 1\. Maximum value of 1000\.  
Required: No

 ** [NextToken](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-NextToken"></a>
A token to specify where to start paginating\. This is the [ListFragments:NextToken](#KinesisVideo-reader_ListFragments-response-NextToken) from a previously truncated response\.  
Type: String  
Length Constraints: Minimum length of 1\.  
Required: No

 ** [StreamName](#API_reader_ListFragments_RequestSyntax) **   <a name="KinesisVideo-reader_ListFragments-request-StreamName"></a>
The name of the stream from which to retrieve a fragment list\.  
Type: String  
Length Constraints: Minimum length of 1\. Maximum length of 256\.  
Pattern: `[a-zA-Z0-9_.-]+`   
Required: Yes

## Response Syntax<a name="API_reader_ListFragments_ResponseSyntax"></a>

```
HTTP/1.1 200
Content-type: application/json

{
   "[Fragments](#KinesisVideo-reader_ListFragments-response-Fragments)": [ 
      { 
         "[FragmentLengthInMilliseconds](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentLengthInMilliseconds)": number,
         "[FragmentNumber](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentNumber)": "string",
         "[FragmentSizeInBytes](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-FragmentSizeInBytes)": number,
         "[ProducerTimestamp](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-ProducerTimestamp)": number,
         "[ServerTimestamp](API_reader_Fragment.md#KinesisVideo-Type-reader_Fragment-ServerTimestamp)": number
      }
   ],
   "[NextToken](#KinesisVideo-reader_ListFragments-response-NextToken)": "string"
}
```

## Response Elements<a name="API_reader_ListFragments_ResponseElements"></a>

If the action is successful, the service sends back an HTTP 200 response\.

The following data is returned in JSON format by the service\.

 ** [Fragments](#API_reader_ListFragments_ResponseSyntax) **   <a name="KinesisVideo-reader_ListFragments-response-Fragments"></a>
A list of fragment numbers that correspond to the time stamp range provided\.  
Type: Array of [Fragment](API_reader_Fragment.md) objects

 ** [NextToken](#API_reader_ListFragments_ResponseSyntax) **   <a name="KinesisVideo-reader_ListFragments-response-NextToken"></a>
If the returned list is truncated, the operation returns this token to use to retrieve the next page of results\. This value is `null` when there are no more results to return\.  
Type: String  
Length Constraints: Minimum length of 1\.

## Errors<a name="API_reader_ListFragments_Errors"></a>

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

## See Also<a name="API_reader_ListFragments_SeeAlso"></a>

For more information about using this API in one of the language\-specific AWS SDKs, see the following:
+  [AWS Command Line Interface](http://docs.aws.amazon.com/goto/aws-cli/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for \.NET](http://docs.aws.amazon.com/goto/DotNetSDKV3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for C\+\+](http://docs.aws.amazon.com/goto/SdkForCpp/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Go](http://docs.aws.amazon.com/goto/SdkForGoV1/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Java](http://docs.aws.amazon.com/goto/SdkForJava/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for JavaScript](http://docs.aws.amazon.com/goto/AWSJavaScriptSDK/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for PHP V3](http://docs.aws.amazon.com/goto/SdkForPHPV3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Python](http://docs.aws.amazon.com/goto/boto3/kinesis-video-reader-data-2017-09-30/ListFragments) 
+  [AWS SDK for Ruby V2](http://docs.aws.amazon.com/goto/SdkForRubyV2/kinesis-video-reader-data-2017-09-30/ListFragments) 