  
---
title: Share the Screen
description: 
platform: Unity
updatedAt: Friday, January 1, 2021 00:55:05  UTC

---
# Screen Sharing
## Introduction

During a video call or interactive streaming, **sharing the screen** enhances communication by displaying the speaker's screen on the display of other speakers or audience members in the channel.

Screen sharing is applied in the following scenarios:

- In a video conference, the speaker can share an image of a local file, web page, or presentation with other users in the channel.
- In an online class, the teacher can share the slides or notes with students.

In Agora Unity SDK, two ways of screen sharing are provided. Namely, **App Screen Sharing** and **Desktop Screen Sharing**.  The follow table compares the two approaches.

|  |Supported Platforms  |Application| Underlining support|
|--|--|--|--
| App|Android,iOS,Windows, MacOS  |Current viewable screen of the Unity App|[external video source](https://docs.agora.io/en/Video/custom_video_unity?platform=Unity)|
|Desktop|Windows, MacOS|Entire desktop or application region/window running on the platform|API function call|



## Implementation

Before sharing the screen, ensure that you have implemented the basic real-time communication functions in your project. See [Start a Video Call](https://docs.agora.io/en/Video/start_call_unity?platform=Unity) or [Start Interactive Video Streaming](https://docs.agora.io/en/Interactive%20Broadcast/start_live_unity?platform=Unity) for details.

You may jump to the API Reference section find the Desktop Sharing APIs if that's what you wanted to do.
  
Refer to the following steps to share the screen via **external video source** in your project:

1. Specify the external video source by calling `SetExternalVideoSource` before `JoinChannelByKey`.

   ```C#
mRtcEngine.SetExternalVideoSource(true, false);
	 ```

2. Define the Texture2D, and use Texture2D to read screen pixels as an external video source.

   ```C#
mRect = new Rect(0, 0, Screen.width, Screen.height);
mTexture = new Texture2D((int)mRect.width, (int)mRect.height, TextureFormat.GRBA32, false);
mTexture.ReadPixels(mRect, 0, 0);
mTexture.Apply();
	 ```
   
3. Call `PushVideoFrame` to send the video source to the SDK and to implement screen sharing.

   ```C#
int a = rtc.PushVideoFrame(externalVideoFrame);
	 ```
   

### API call sequence

The following diagram shows the API call sequence for sharing the screen via external video source.

![](https://web-cdn.agora.io/docs-files/1576229371972)

### Sample code

Refer to the following code to share the screen in your project.

```C#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using agora_gaming_rtc;
using UnityEngine.UI;
using System.Globalization;
using System.Runtime.InteropServices;
using System;

public class ShareScreen : MonoBehaviour
{
   Texture2D mTexture;
   Rect mRect;
   [SerializeField]
   private string appId = "Your_AppID";
   [SerializeField]
   private string channelName = "agora";
   public IRtcEngine mRtcEngine;
   int i = 100;
	
   void Start()
   {
       Debug.Log("ScreenShare Activated");
       mRtcEngine = IRtcEngine.GetEngine(appId);
       // Sets the output log level of the SDK.
       mRtcEngine.SetLogFilter(LOG_FILTER.DEBUG | LOG_FILTER.INFO | LOG_FILTER.WARNING | LOG_FILTER.ERROR | LOG_FILTER.CRITICAL);
       // Enables the video module.
       mRtcEngine.EnableVideo();
       // Enables the video observer.
       mRtcEngine.EnableVideoObserver();
       // Configures the external video source.
       mRtcEngine.SetExternalVideoSource(true, false);
       // Joins a channel.
       mRtcEngine.JoinChannel(channelName, null, 0);
       // Creates a rectangular region of the screen.
       mRect = new Rect(0, 0, Screen.width, Screen.height);
       // Creates a texture of the rectangle you create.
       mTexture = new Texture2D((int)mRect.width, (int)mRect.height, TextureFormat.GRBA32, false);
   }

   void Update()
   {
       StartCoroutine(shareScreen());
   }
			 
   // Starts to share the screen.
   IEnumerator shareScreen()
   {
       yield return new WaitForEndOfFrame();
       // Reads the pixels of the rectangle you create.
       mTexture.ReadPixels(mRect, 0, 0);
       // Applies the pixels read from the rectangle to the texture.
       mTexture.Apply();
       // Gets the raw texture data and apply it to an array of bytes.
       byte[] bytes = mTexture.GetRawTextureData();
       // Gives enough space for the bytes array.
       int size = Marshal.SizeOf(bytes[0]) * bytes.Length;
       // Checks whether the IRtcEngine instance exists.
       IRtcEngine rtc = IRtcEngine.QueryEngine();
       if (rtc != null)
       {
           // Creates a new external video frame.
           ExternalVideoFrame externalVideoFrame = new ExternalVideoFrame();
           // Sets the buffer type of the video frame.
           externalVideoFrame.type = ExternalVideoFrame.VIDEO_BUFFER_TYPE.VIDEO_BUFFER_RAW_DATA;
           // Sets the format of the video pixel.
           externalVideoFrame.format = ExternalVideoFrame.VIDEO_PIXEL_FORMAT.VIDEO_PIXEL_GRBA;
           // Applies the raw data.
           externalVideoFrame.buffer = bytes;
           // Sets the width (pixel) of the video frame.
           externalVideoFrame.stride = (int)mRect.width;
           // Sets the height (pixel) of the video frame.
           externalVideoFrame.height = (int)mRect.height;
           // Removes pixels from the sides of the frame
           externalVideoFrame.cropLeft = 10;
           externalVideoFrame.cropTop = 10;
           externalVideoFrame.cropRight = 10;
           externalVideoFrame.cropBottom = 10;
           // Rotates the video frame (0, 90, 180, or 270)
           externalVideoFrame.rotation = 180;
           // Increments i with the video timestamp.
           externalVideoFrame.timestamp = i++;
           // Pushes the external video frame with the frame you create.
           int a = rtc.PushVideoFrame(externalVideoFrame);
       }
   }
}
```

### API reference

- [SetExternalVideoSource](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#aae4a31d2375ed620605360ae1199eee8)
- [PushVideoFrame](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#af9e8d34e2a1ac07b8984fb6181a6ab81)
- [StartScreenCaptureByDisplayId](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a9fa9dd48fc3ecf616d39f6fdf00000c2)
- [StartScreenCaptureByScreenRect](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a7cc7cb7e1edc249e42c7e171ec85d7a5)
- [StartScreenCaptureByWindowId](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#aab2aacc7ef350f54010002fcfe2554ae)
- [StopScreenCapture](https://docs.agora.io/en/Video/API%20Reference/unity/classagora__gaming__rtc_1_1_i_rtc_engine.html#a58ee4ff8e3de98b0b13571031904d2ba)



## GitHub Resource
[Agora Advanced Demo for Unity](https://github.com/AgoraIO/Agora-Unity-Quickstart/tree/master/Advanced-Demos)

