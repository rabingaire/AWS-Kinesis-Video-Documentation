# Troubleshooting Kinesis Video Streams<a name="troubleshooting"></a>

Use the following information to troubleshoot common issues encountered with Amazon Kinesis Video Streams\.

**Topics**
+ [Troubleshooting General Issues](#troubleshooting-general)
+ [Troubleshooting API Issues](#troubleshooting-api)
+ [Troubleshooting Java Issues](#troubleshooting-java)
+ [Troubleshooting Producer Library Issues](#troubleshooting-producer)
+ [Troubleshooting Stream Parser Library Issues](#troubleshooting-parser)

## Troubleshooting General Issues<a name="troubleshooting-general"></a>

This section describes general issues that you might encounter when working with Kinesis Video Streams\.

**Topics**
+ [Latency too high](#troubleshooting-general-latency)

### Latency too high<a name="troubleshooting-general-latency"></a>

Latency might be caused by the duration of fragments that are sent to the Kinesis Video Streams service\. One way to reduce the latency between the producer and the service is to configure the media pipeline to produce shorter fragment durations\.

To reduce the number of frames sent in each fragment, and thus reduce the amount of time for each fragment, reduce the following value in `kinesis_video_gstreamer_sample_app.cpp`:

```
g_object_set(G_OBJECT (data.encoder), "bframes", 0, "key-int-max", 45, "bitrate", 512, NULL);
```

## Troubleshooting API Issues<a name="troubleshooting-api"></a>

This section describes API issues that you might encounter when working with Kinesis Video Streams\.

**Topics**
+ [Error: “Unable to determine service/operation name to be authorized”](#troubleshooting-api-name-auth)
+ [Error: “Failed to put a frame in the stream”](#troubleshooting-api-putframe)
+ [Error: “Service closed connection before final AckEvent was received”](#troubleshooting-api-closeconnection)

### Error: “Unable to determine service/operation name to be authorized”<a name="troubleshooting-api-name-auth"></a>

`GetMedia` can fail with the following error:

```
Unable to determine service/operation name to be authorized
```

This error might occur if the endpoint is not properly specified\. When you are getting the endpoint, be sure to include the following parameter in the `GetDataEndpoint` call, depending on the API to be called:

```
--api-name GET_MEDIA
--api-name PUT_MEDIA
--api-name GET_MEDIA_FOR_FRAGMENT_LIST
--api-name LIST_FRAGMENTS
```

### Error: “Failed to put a frame in the stream”<a name="troubleshooting-api-putframe"></a>

`PutMedia` can fail with the following error:

```
Failed to put a frame in the stream
```

This error might occur if connectivity or permissions are not available to the service\. Run the following in the AWS CLI, and verify that the stream information can be retrieved:

```
aws kinesisvideo describe-stream --stream-name StreamName --endpoint https://ServiceEndpoint.kinesisvideo.region.amazonaws.com
```

If the call fails, see [Troubleshooting AWS CLI Errors](http://docs.aws.amazon.com/cli/latest/userguide/troubleshooting.html) for more information\.

### Error: “Service closed connection before final AckEvent was received”<a name="troubleshooting-api-closeconnection"></a>

`PutMedia` can fail with the following error:

```
com.amazonaws.SdkClientException: Service closed connection before final AckEvent was received
```

This error might occur if `PushbackInputStream` is improperly implemented\. Ensure that the `unread()` methods are correctly implemented\.

## Troubleshooting Java Issues<a name="troubleshooting-java"></a>

This section describes how to troubleshoot common Java issues encountered when working with Kinesis Video Streams\.

**Topics**
+ [Enabling Java logs](#troubleshooting-java-log)

### Enabling Java logs<a name="troubleshooting-java-log"></a>

To troubleshoot issues with Java samples and libraries, it is helpful to enable and examine the debug logs\. To enable debug logs, do the following:

1. Add `log4j` to the `pom.xml``` file, in the `dependencies` node: 

   ```
   <dependency>
       <groupId>log4j</groupId>
       <artifactId>log4j</artifactId>
       <version>1.2.17</version>
   </dependency>
   ```

1. In the `target/classes` directory, create a file named `log4j.properties` with the following contents:

   ```
   # Root logger option
   log4j.rootLogger=DEBUG, stdout
   
   # Redirect log messages to console
   log4j.appender.stdout=org.apache.log4j.ConsoleAppender
   log4j.appender.stdout.Target=System.out
   log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
   log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L - %m%n
   
   log4j.logger.org.apache.http.wire=DEBUG
   ```

The debug logs then print to the IDE console\.

## Troubleshooting Producer Library Issues<a name="troubleshooting-producer"></a>

This section describes issues that you might encounter when using the [Producer Libraries](producer-sdk.md)\.

**Topics**
+ [Cannot compile the Producer SDK](#troubleshooting-producer-compile)
+ [Video stream does not appear in the console](#troubleshooting-producer-console)
+ [Error: "Security token included in the request is invalid" when streaming data using the GStreamer demo application](#troubleshooting-producer-general-securitytoken)
+ [Error: "Failed to submit frame to Kinesis Video client"](#troubleshooting-producer-failed-frame-client)
+ [GStreamer application stops with "streaming stopped, reason not\-negotiated" message on OS X](#troubleshooting-producer-failed-stream-osx)
+ [Error: "Failed to allocate heap" when creating Kinesis Video Client in GStreamer demo on Raspberry Pi](#troubleshooting-producer-raspberrypi-heap)
+ [Error: "Illegal Instruction" when running GStreamer demo on Raspberry Pi](#troubleshooting-producer-raspberrypi-illegalinstruction)
+ [Camera fails to load on Raspberry Pi](#troubleshooting-producer-raspberrypi-camera)
+ [Camera can't be found on macOS High Sierra](#troubleshooting-producer-sierra-camera)
+ [jni\.h file not found when compiling on macOS High Sierra](#troubleshooting-producer-sierra-compile)
+ [Curl errors when running the GStreamer demo app](#troubleshooting-producer-curl)
+ [Time stamp/range assertion at run time on Raspberry Pi](#troubleshooting-producer-raspberrypi-timestamp-assert)
+ [Assertion on gst\_value\_set\_fraction\_range\_full on Raspberry Pi](#troubleshooting-producer-raspberrypi-gst-assert)

### Cannot compile the Producer SDK<a name="troubleshooting-producer-compile"></a>

Verify that the required libraries are in your path\. To verify this, use the following command:

```
$ env | grep LD_LIBRARY_PATH
LD_LIBRARY_PATH=/home/local/awslabs/amazon-kinesis-video-streams-producer-sdk-cpp/kinesis-video-native-build/downloads/local/lib
```

### Video stream does not appear in the console<a name="troubleshooting-producer-console"></a>

To display your video stream in the console, it must be encoded using H\.264 in AvCC format\. If your stream is not displayed, verify the following:
+ Your [NAL Adaptation Flags](producer-reference-nal.md) are set to `NAL_ADAPTATION_ANNEXB_NALS | NAL_ADAPTATION_ANNEXB_CPD_NALS` if the original stream is in Annex\-B format\. This is the default value in the `StreamDefinition` constructor\.
+ You are providing the codec private data correctly\. For H\.264, this is the sequence parameter set \(SPS\) and picture parameter set \(PPS\)\. Depending on your media source, this data may be retrieved from the media source separately or encoded into the frame\.

  Many elementary streams are in the following format, where `Ab` is the Annex\-B start code \(001 or 0001\):

  ```
  Ab(Sps)Ab(Pps)Ab(I-frame)Ab(P/B-frame) Ab(P/B-frame)…. Ab(Sps)Ab(Pps)Ab(I-frame)Ab(P/B-frame) Ab(P/B-frame)
  ```

  The CPD \(Codec Private Data\) which in the case of H\.264 is in the stream as SPS and PPS, can be adapted to the AvCC format\. Unless the media pipeline gives the CPD separately, the application can extract the CPD from the frame by looking for the first Idr frame \(which should contain the SPS/PPS\), extract the two NALUs \[which will be Ab\(Sps\)Ab\(Pps\)\] and set it in the CPD in `StreamDefinition`\.

### Error: "Security token included in the request is invalid" when streaming data using the GStreamer demo application<a name="troubleshooting-producer-general-securitytoken"></a>

If this error occurs, there is an issue with your credentials\. Verify the following:
+ If you are using temporary credentials, you must specify the session token\.
+ Verify that your temporary credentials are not expired\.
+ Verify that you have the proper rights set up\.
+ On macOS, verify that you do not have credentials cached in Keychain\.

### Error: "Failed to submit frame to Kinesis Video client"<a name="troubleshooting-producer-failed-frame-client"></a>

If this error occurs, the time stamps are not properly set in the source stream\. Try the following:
+ Use the latest SDK sample, which might have an update that fixes your issue\.
+ Set the high\-quality stream to a higher bit rate, and fix any jitter in the source stream if the camera supports doing so\.

### GStreamer application stops with "streaming stopped, reason not\-negotiated" message on OS X<a name="troubleshooting-producer-failed-stream-osx"></a>

Streaming may stop on OS X with the following message:

```
Debugging information: gstbasesrc.c(2939): void gst_base_src_loop(GstPad *) (): /GstPipeline:test-pipeline/GstAutoVideoSrc:source/GstAVFVideoSrc:source-actual-src-avfvide:
streaming stopped, reason not-negotiated (-4)
```

A possible workaround for this is to remove the framerate parameters from the `gst_caps_new_simple` call in `kinesis_video_gstreamer_sample_app.cpp`:

```
GstCaps *h264_caps = gst_caps_new_simple("video/x-h264",
                                             "profile", G_TYPE_STRING, "baseline",
                                             "stream-format", G_TYPE_STRING, "avc",
                                             "alignment", G_TYPE_STRING, "au",
                                             "width", GST_TYPE_INT_RANGE, 320, 1920,
                                             "height", GST_TYPE_INT_RANGE, 240, 1080,
                                             "framerate", GST_TYPE_FRACTION_RANGE, 0, 1, 30, 1,
                                             NULL);
```

### Error: "Failed to allocate heap" when creating Kinesis Video Client in GStreamer demo on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-heap"></a>

The GStreamer sample application tries to allocate 512 MB of RAM, which might not be available on your system\. You can reduce this allocation by reducing the following value in `KinesisVideoProducer.cpp`:

```
device_info.storageInfo.storageSize = 512 * 1024 * 1024;
```

### Error: "Illegal Instruction" when running GStreamer demo on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-illegalinstruction"></a>

If you encounter the following error when executing the GStreamer demo, ensure that you have compiled the application for the correct version of your device\. \(For example, ensure that you are not compiling for Raspberry Pi 3 when you are running on Raspberry Pi 2\.\)

```
INFO - Initializing curl.
Illegal instruction
```

### Camera fails to load on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-camera"></a>

To check whether the camera is loaded, run the following:

```
$ ls /dev/video*
```

If nothing is found, run the following:

```
$ vcgencmd get_camera
```

The output should look similar to the following:

```
supported=1 detected=1
```

If the driver does not detect the camera, do the following:

1. Check the physical camera setup and verify that it's connected properly\.

1. Run the following to upgrade the firmware:

   ```
   $ sudo rpi-update
   ```

1. Restart the device\.

1. Run the following to load the driver:

   ```
   $ sudo modprobe bcm2835-v4l2
   ```

1. Verify that the camera was detected:

   ```
   $ ls /dev/video*
   ```

### Camera can't be found on macOS High Sierra<a name="troubleshooting-producer-sierra-camera"></a>

On macOS High Sierra, the demo application can't find the camera if more than one camera is available\.

### jni\.h file not found when compiling on macOS High Sierra<a name="troubleshooting-producer-sierra-compile"></a>

To resolve this error, update your installation of Xcode to the latest version\.

### Curl errors when running the GStreamer demo app<a name="troubleshooting-producer-curl"></a>

To resolve curl errors when you run the GStreamer demo application, copy [this certificate file](https://www.amazontrust.com/repository/SFSRootCAG2.pem) to `/etc/ssl/cert.pem`\.

### Time stamp/range assertion at run time on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-timestamp-assert"></a>

If a time stamp range assertion occurs at run time, update the firmware and restart the device:

```
$ sudo rpi-update 
$ sudo reboot
```

### Assertion on gst\_value\_set\_fraction\_range\_full on Raspberry Pi<a name="troubleshooting-producer-raspberrypi-gst-assert"></a>

The following assertion appears if the `uv4l` service is running: 

```
gst_util_fraction_compare (numerator_start, denominator_start, numerator_end, denominator_end) < 0' failed
```

If this occurs, stop the `uv4l` service and restart the application\.

## Troubleshooting Stream Parser Library Issues<a name="troubleshooting-parser"></a>

This section describes issues that you might encounter when using the [Stream Parser Library](parser-library.md)\.

**Topics**
+ [Cannot access a single frame from the stream](#troubleshooting-parser-frame)
+ [Fragment decoding error](#troubleshooting-parser-fragment)

### Cannot access a single frame from the stream<a name="troubleshooting-parser-frame"></a>

To access a single frame from a streaming source in your consumer application, ensure that your stream contains the correct codec private data\. For information about the format of the data in a stream, see [Data Model](how-data.md)\. 

To learn how to use codec private data to access a frame, see the following test file on the GitHub website: [KinesisVideoRendererExampleTest\.java](https://github.com/aws/amazon-kinesis-video-streams-parser-library/blob/master/src/test/java/com/amazonaws/kinesisvideo/parser/examples/KinesisVideoRendererExampleTest.java)

### Fragment decoding error<a name="troubleshooting-parser-fragment"></a>

If your fragments are not properly encoded in an H\.264 format and level that the browser supports, you might see the following error when playing your stream in the console:

```
Fragment Decoding Error
There was an error decoding the video data. Verify that the stream contains valid H.264 content
```

If this occurs, verify the following:
+ The resolution of the frames matches the resolution specified in the Codec Private Data\.
+ The H\.264 profile and level of the encoded frames matches the profile and level specified in the Codec Private Data\.
+ The browser supports the profile/level combination\. Most current browsers support all profile and level combinations\.
+ The time stamps are accurate and in the correct order, and no duplicate time stamps are being created\.
+ Your application is encoding the frame data using the H\.264 format\.