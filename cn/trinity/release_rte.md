
---
title: 发版说明
description: 
platform: All Platforms
updatedAt: Thu Apr 29 2021 12:27:23 GMT+0800 (CST)
---
# 发版说明
本页提供 Agora 下一代 RTC SDK 的发版说明。

## v3.4.200

该版本于 2021 年 4 月 29 日发布。

#### 行为变更

**音频场景和属性**

该版本将 `audioScenario` 默认值改为 `AUDIO_SCENARIO_HIGH_DEFINITION`，且删除包含 `profile` 和 `scenario` 参数的 `setAudioProfile` 方法。Agora 推荐你在初始化 `RtcEngine` 时通过 `audioScenario` 设置音频场景，通过只包含 `profile` 参数的 `setAudioProfile` 方法设置音频属性。

#### 新增特性

**1. 媒体播放器**

该版本支持媒体播放器功能，帮助开发者在实时音视频互动中实现播放本地和在线媒体资源，并允许用户独自观看或与其他用户共享。

你需要调用 `createMediaPlayer` 方法创建媒体播放器对象，并通过如下接口实现播放器功能并监听播放器事件。

<table>
<thead>
  <tr>
    <th></th>
    <th>Android</th>
    <th>iOS</th>
    <th>Windows</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>方法</td>
    <td><code>IMediaPlayer</code></td>
    <td><code>AgoraRtcMediaPlayerProtocol</code><br></td>
    <td><code>IMediaPlayer</code><br></td>
  </tr>
  <tr>
    <td>回调</td>
    <td><code>IMediaPlayerObserver</code></td>
    <td><code>AgoraRtcMediaPlayerDelegate</code><br></td>
    <td><code>IMediaPlayerSourceObserver</code></td>
  </tr>
</tbody>
</table>

**2. Extension**

自该版本起，Agora SDK 与第三方合作，将音视频前处理、后处理、自渲染等功能封装成 Extension 供应用开发者使用。

**3. 视频双流模式**

视频双流指高分辨率、高帧率的的视频大流和低分辨率、低帧率的的视频小流。Agora 默认发送、接收视频大流，你也可以调用 `enableDualStreamMode` 开启视频双流模式，以方便弱网用户发送、接收视频小流。

该版本优化视频双流模式，新增两个与原方法同名的 `enableDualStreamMode` 方法：

- 包含 `sourceType` 参数的同名方法：相比原方法，可以设置视频源的类型，如视频来源于摄像头、屏幕、自采集。
- 包含 `sourceType` 和 `streamConfig` 参数的同名方法：相比只包含 `sourceType` 参数的同名方法，可以设置视频小流的配置，如视频尺寸、码率、帧率。

**4. 设置日志文件**

为保证日志内容的完整性，该版本在 `RtcEngineContext` 中新增 `logConfig` 成员变量，在你初始化 `RtcEngine` 时可用于设置 Agora SDK 输出的日志文件。‘

自该版本起，Agora 不推荐使用 `setLogFile`、`setLogFileSize`、`setLogFilter`、`setLogLevel` 方法设置日志文件。

**5. 加密**

该版本新增 `enableEncryption` 方法，用于对频道内媒体流进行国密 SM4 加密。加解密失败时，Agora SDK 会触发 `onEncryptionError` 回调。

**6. 删除指定事件句柄 (Android)**

在特定场景下，开发者不想再接收某些事件的回调。该版本新增 `removeHandler` 方法，你可以调用该方法删除不再需要的事件句柄。

**7. 客户端录音**

为在录音时设置录音内容，该版本新增 `startAudioRecording` 方法并废弃同名原方法。通过新方法的 `config` 参数，你可以设置录音音质、内容、采样率等。

**8. 调节本地播放的指定远端用户音量**

该版本新增 `adjustUserPlaybackSignalVolume` 方法，用以调节本地用户听到的指定远端用户的音量。通话或直播过程中，你可以多次调用该方法，来调节多个远端用户在本地播放的音量，或对某个远端用户在本地播放的音量调节多次。

**9. 视频采集旋转 (Windows)**

该版本新增 `setCameraDeviceOrientation` 方法，支持你在用户设备不带重力感应功能时，手动调整采集到的视频画面的旋转角度。

**10. 多设备采集 (Windows)**

为满足用户对使用多摄像头、多屏幕采集发送视频的需求，该版本新增如下方法：

- `startPrimaryCameraCapture`: 开始通过第一个摄像头采集视频。
- `startSecondaryCameraCapture`: 开始通过第二个摄像头采集视频。
- `startPrimaryScreenCapture`: 开始采集共享第一个屏幕。
- `startSecondaryScreenCapture`: 开始采集共享第二个屏幕。

**11. 设备权限出错回调 (Android/iOS)**

自该版本起，Agora SDK 新增 `onPermissionError` 回调，及时向用户报告应用无法获取设备权限。用户可以通过 `permissionType` 参数了解应用受限的设备权限，并按需开放权限。

**12. 上行网络信息改变回调**

该版本新增 `onUplinkNetworkInfoUpdated` 回调，报告用户的上行网络信息发生改变，如视频编码的目标码率。

**13. 本地视频首帧发布回调**

该版本新增 `onFirstLocalVideoFramePublished` 回调，报告用户已发布本地视频首帧。


#### 改进

**1. 多频道回调**

在多频道场景下，为方便开发者了解回调事件对应的 connection ID，该版本新增 `IRtcEngineEventHandlerEx` 类。

**2. 视频编码降级偏好**

Agora SDK 允许你通过 `degradationPreference` 设置带宽受限时本地视频编码降级偏好，如降低视频帧率保障视频质量，降低视频质量保障视频帧率。自该版本起，`degradationPreference` 新增支持设为 `MAINTAIN_BALANCED`，弱网下会降低视频帧率和视频质量，以在流畅性和视频质量之间取得平衡，适用于流畅性和画质均优先的场景，如一对一通话、一对一教学、多人会议。

**3. 耳返 (Android/iOS)**

为提升耳返的用户体验，该版本新增一个包含 `includeAudioFilter` 参数的 `enableInEarMonitoring` 方法，允许你设置耳返声音经过降噪或美声变声等处理。

**4. 创建数据流**

为了支持歌词同步、课件同步等场景，该版本废弃了原有的 `createDataStream` 方法，并使用新的同名方法替代，用于创建数据流，并设置数据流是否与发布到 Agora 频道内的音频流同步以及接收到的数据是否有序。

**5. Echo 测试**

Echo 测试指用户测试音频设备（耳麦、扬声器等）和网络连接是否正常。该版本新增 `startEchoTest` 方法并废弃同名原方法，你可以通过新方法的 `intervalInSeconds` 参数设置 SDK 返回 Echo 测试结果的时间间隔。

**6. 质量透明**

该版本进一步扩充了实时音视频互动中的质量数据：

- 在 `RtcStats` 中新增 `txPacketLossRate`: 报告网络对抗前，本地客户端到边缘服务器的丢包率 (%)。
- 在 `RtcStats` 中新增 `rxPacketLossRate`: 报告网络对抗前，边缘服务器到本地客户端的丢包率 (%)。
- 在 `RemoteVideoStats` 中新增 `avSyncTimeMs`: 报告实时音视频互动过程中，音频超前视频的时间 (ms)。

## v3.3.204

该版本于 2021 年 3 月 25 日发布。

#### 新增特性

**屏幕共享**

为方便开发者在 Android 平台上实现屏幕共享，该版本新增 `startScreenCapture` 方法。你可以调用该方法，并使用 Android 系统原生的 `MediaProjection` 开启全屏共享。

#### 改进

**离开频道（高级选项）**

为防止用户离开频道时音效播放被打断，该版本在 `LeaveChannelOptions` 结构体中新增 `stopAllEffect` 成员。你可以通过该成员设置是否在用户离开频道时停止播放音效。

## v3.3.202

该版本于 2021 年 3 月 5 日发布。

#### 新增特性

**1. String 型用户名**

很多 App 使用 String 类型的用户名。为降低开发成本，Agora 新增支持 String 型的 User Account，方便用户通过如下接口直接使用 App 账号加入 Agora 频道：

- `registerLocalUserAccount`
- `joinChannelWithUserAccount`

对于其他接口，Agora 沿用 Int 型的 UID。Agora Engine 会维护 UID 和 User Account 映射表，你可以随时通过 User Account 获取 UID，或者通过 UID 获取 User Account，无需自己维护映射表。

为保证通信质量，频道内所有用户需使用同一数据类型的用户名，即频道内的所有用户名应同为 Int 型或同为 String 型。

<div class="alert note"><ul><li>同一频道内，Int 型的 UID 和 String 型的 User Account 不可混用。</li><li>如果你的频道内有不支持 String 型 User Account 的用户，则只能使用 Int 型的 UID。</li><li>如果使用 String 型的 User Account，请确保你的服务端用户生成 Token 的脚本已升级至最新版本。</li></ul></div>

**2. 音频设备测试**

为方便你在通话前测试音频设备是否正常工作，该版本新增如下方法：

- `startRecordingDeviceTest`: 开始音频采集设备测试。
- `startPlaybackDeviceTest`: 开始音频播放设备测试。
- `startAudioDeviceLoopbackTest`: 开始音频采集设备和播放设备测试。

#### 改进

**1. 端到端网络延迟**

该版本新增在 RemoteAudioStats 中新增 networkTransportDelay，以报告发送端到接收端的网络延迟（毫秒）。

![](https://web-cdn.agora.io/docs-files/1565595137741)

**2. 发布和订阅状态改变回调**

该版本新增以下回调方便你了解音视频流当前的发布及订阅状态，有助于订阅和发布相关的数据统计：

- `onAudioPublishStateChanged`: 音频发布状态发生改变。
- `onVideoPublishStateChanged`: 视频发布状态发生改变。
- `onAudioSubscribeStateChanged`: 音频订阅状态发生改变。
- `onVideoSubscribeStateChanged`: 视频订阅状态发生改变。

## v3.1.226

该版本于 2020 年 12 月 8 日发布。

#### 新增特性

**1. 镜像模式**

为提升视频镜像的使用体验，该版本增加了视频编码镜像和视频渲染镜像的功能：

- 视频编码镜像：在 `VideoEncoderConfiguration` 结构体中，新增 `mirrorMode` 成员，方便设置本地视频编码的镜像模式，即远端看本地是否镜像。
- 视频渲染镜像：在 `VideoCanvas` 结构体中，新增 `mirrorMode` 成员，方便用户在调用 `setupLocalVideo` 方法初始化本地视图时，设置本地看本地是否镜像，以及调用 `setupRemoteVideo` 方法初始化远端视图时，设置本地看远端是否镜像；同时在 `setLocalRenderMode` 和 `setRemoteRenderMode` 方法中新增 `mirrorMode` 参数，支持在通话中更新本地看本地，或本地看远端的镜像模式。

**2. 跨频道媒体流转发**

跨频道媒体流转发，指将主播的媒体流转发至其他直播频道，实现主播跨频道与其他主播实时互动的场景。该版本新增如下接口，通过将源频道中的媒体流转发至目标频道，实现跨直播间连麦功能：

- `startChannelMediaRelay`
- `updateChannelMediaRelay`
- `stopChannelMediaRelay`

在跨频道媒体流转发过程中，SDK 会通过 `onChannelMediaRelayStateChanged` 和 `onChannelMediaRelayEvent` 回调报告媒体流转发的状态和事件。

**3. 离开频道（高级选项）**

为防止用户离开频道时音频播放或采集被打断，该版本新增一个含有 `options` 参数的 `leaveChannel` 方法。通过 `options` 参数，你可以设置如下高级选项：

- 用户离开频道时，设置是否停止播放混音音乐文件。
- 用户离开频道时，设置是否停止麦克风采集音频。

**4. 切换前后摄像头**

该版本新增支持设置前置或后置摄像头采集视频。你可以调用 `setCameraCapturerConfiguration` 方法设置：

- 前置摄像头：`CAMERA_REAR(0)`。
- 后置摄像头：`CAMERA_FRONT(1)`。

**5. 推流到 CDN**

为满足单主播和多主播的 CDN 直播推流需求，该版本新增如下接口，全面支持推送声网频道内的音视频流到 CDN：

- `setLiveTranscoding`: 设置 CDN 直播推流的转码属性。
- `addPublishStreamUrl`: 开始 CDN 直播推流。你需要传入 CDN 推流地址，并设置是否开启转码。
- `removePublishStreamUrl`: 结束 CDN 直播推流。

#### 改进

**1. 视频帧观测器**

为丰富视频帧观测器用法，该版本在 `IVideoFrameObserver` 类中增如下成员函数：

- `getVideoFrameProcessMode`: 从视频帧观测器拿到视频帧后，你可以通过该方法设置对视频帧的处理方式：
    - 只读模式：你只读取数据，不修改数据。视频帧观测器相当于一个渲染器。
    - 读写模式：你既读取数据，也修改数据。视频帧观测器相当于一个 video filter。
- `onScreenCaptureVideoFrame`: 获取屏幕共享视频流的视频帧。成功注册视频帧观测器后，SDK 会在每次获取到一帧屏幕共享流的视频帧时，通过该回调向你抛出视频帧。

**2. 区域访问限制**

该版本新增支持限制访问 Agora 服务器所在区域为仅日本或仅印度。

## v3.1.214

该版本于 2020 年 9 月 24 日发布。

**功能亮点**

#### 1. 加强频道管理

- 支持多次进入相同频道（用 connection id 标示，用于发送多流进入相同频道）。
- 支持进入不同频道用于订阅不同频道的流。
- 增加频道媒体选项。

#### 2. 支持多路媒体流

- 支持发布多条外部视频流进入“相同”/“不同”频道 ，包括 摄像头采集，屏幕采集和自渲染。
- 支持发布多路外部音频 PCM 流进入“相同”/“不同”的频道 ，包括麦克风和自采集。
- 支持多路音频流自动混音。

#### 3. 其他功能

- 支持 `CHANNEL_PROFILE_CLOUD_GAMING`, 适合云游戏场景，端到端延迟低至 20 ms。
- Android 支持 Texture 自采集，自渲染。
- Android 支持 EGL renderer，支持全链路硬件处理优化，包括硬件采集，编解码，渲染，旋转处理和大幅优化 CPU。

**新增特性**

#### 1. 音效

为方便用户在通话中添加音效，该版本新增如下 API。

预加载音效：

- `preloadEffect`: 将音效文件预加载至内存。
- `unloadEffect`: 从内存释放指定的预加载音效文件。
- `unloadAllEffects`: 从内存释放所有预加载音效文件。

管理播放状态：

- `playEffect`: 播放指定音效文件。你可以设置是否将音频发布到远端，让本地和远端用户都可听到音效。
- `playAllEffects`: 播放所有音效文件。你可以设置是否将音频发布到远端，让本地和远端用户都可听到音效。
- `pauseEffect`: 暂停播放指定音效文件。
- `pauseAllEffects`: 暂停播放所有音效文件。
- `resumeEffect`: 恢复播放指定音效文件。
- `resumeAllEffects`: 恢复播放所有音效文件。
- `stopEffect`: 停止播放指定音效文件。
- `stopAllEffects`: 停止播放所有音效文件。

管理播放音量：

- `getVolumeOfEffect`: 获取指定音效文件的播放音量。
- `setVolumeOfEffect`: 设置指定音效文件的播放音量。
- `getEffectsVolume`: 获取所有音效文件的播放音量。
- `setEffectsVolume`: 获取所有音效文件的播放音量。

#### 2. 设置区域访问限制

该版本支持在初始化 `RtcEngine` 时通过 `areaCode` 成员指定 Agora 服务器的访问区域。该功能为高级功能，适用于有访问安全限制的场景。目前支持的区域有中国大陆、北美、欧洲、亚洲（中国大陆除外）和全球（默认）。

#### 3. 视频质量透明

为方便开发者了解本地和远端的视频质量和状态，该版本新增如下 API：

- `onLocalVideoStats`: 通话中，每 2 秒报告一次本地用户视频流的质量信息，如编码和发送时的码率、帧率和分辨率。
- `onRemoteVideoStats`: 通话中，每 2 秒报告一次接收到的远端视频流的质量信息，如视频码率、丢包率和卡顿时长。
- `onLocalVideoStateChanged`: 本地视频状态改变回调。状态为 `FAILED(3)` 时，请根据错误码排查问题。
- `onRemoteVideoStateChanged`: 远端视频状态改变回调。

#### 4. 发送和接收媒体附属信息

为满足多样化的直播互动需求，如支持主播向观众发放购物链接、电子优惠券和在线测试题，该版本新增支持发送和接收媒体附属信息 (metadata)。你需要自行实现 `IMetadataObserver` 并注册 metadata 观测器。

#### 5. 数据流

该版本新增以下 API，支持使用数据流：

- `createDataStream`: 创建数据流。
- `sendStreamMessage`: 发送数据流。Agora SDK 对发送数据流消息有以下限制：
    - 消息频率：每秒最多可以发送 60 条消息。
    - 消息大小：每条消息不能超过 1 KB。
    - 消息吞吐量：每秒最多可以发送 30 KB 大小的消息。
- `onStreamMessage`: 接收到对方数据流回调。
- `onStreamMessageError`: 接收对方数据流发生错误回调。

#### 6. 设置日志输出等级

该版本新增 `setLogLevel` 方法，支持设置 SDK 输出日志的等级：

- `LOG_LEVEL_NONE (0x0000)`: 不输出任何日志。
- `LOG_LEVEL_INFO (0x0001)`: （推荐值）输出 INFO 等级的日志。
- `LOG_LEVEL_WARN (0x0002)`: 输出 WARN 等级的日志。
- `LOG_LEVEL_ERROR (0x0004)`: 输出 ERROR 等级的日志。
- `LOG_LEVEL_FATAL (0x0008)`: 输出 FATAL 等级的日志。

#### 7. 自定义数据上报

该版本支持自定义数据上报。如需试用，请联系 sales@agora.io 开通并商定自定义数据格式。

#### 8. 视频原始数据（Android）

为方便开发者拿到视频裸数据，该版本新增 `registerVideoFrameObserver` 方法供你注册视频观测器，成功注册后，你可以从 `onCaptureVideoFrame` 中拿到本地采集到的视频裸数据，也可以从 `onRenderVideoFrame` 中拿到接收到的远端用户的视频裸数据。


**改进**

#### 自定义视频源（Android）

该版本优化了 Android 平台的自定义视频源功能。通过 `setExternalVideoSource` 设置自定义视频源后，你可以通过 `pushExternalVideoFrame` 将视频帧推送给 SDK。

该版本提供两个同名的 `pushExternalVideoFrame`，一个使用 `VideoFrame`，一个使用 `AgoraVideoFrame`。如果你想让 SDK 更好地发挥设备的硬件性能，Agora 推荐你使用带 `VideoFrame` 的 `pushExternalVideoFrame` 方法。

**变更**

#### 音频编码属性

该版本废弃原来的 `setAudioProfile` 方法并新增同名的方法。自该版本起，Agora 推荐你通过如下方式设置音频编码属性：

- 创建 `RtcEngine` 时设置音频 `scenario`。
- 调用新的 `setAudioProfile` 方法设置音频 `profile`。

该版本对音频 `scenario` 和 `profile` 进行如下优化：

- `scenario`: 新增 `AUDIO_SCENARIO_HIGH_DEFINITION(6)` 类型，代表高音质场景。Agora 推荐你在 `AUDIO_SCENARIO_DEFAULT(0)`、`AUDIO_SCENARIO_GAME_STREAMING(3)` 和 `AUDIO_SCENARIO_HIGH_DEFINITION(6)` 三种类型中选择一种使用。
- `profile`: 该版本调整了一些 profile 使用的编码码率最大值。

    |profile|旧版本码率 (Kbps)| 当前版本码率 (Kbps)|
    |------|-------|--------|
    |`DEFAULT(0)`|52（直播场景）| 64（直播场景）|
    |`MUSIC_STANDARD(2)`|48|64|
    |`MUSIC_STANDARD_STEREO(3)`|64|80|
    |`MUSIC_HIGH_QUALITY(4)`|128|96|
    |`MUSIC_HIGH_QUALITY_STEREO(5)`|192|128|

**已知问题**

- VQC 算法的问题：当前策略为帧率优先，不能画质优先，因此，在相同弱网下，画质可能不如主版。
- 弱网多人场景的问题：
    - 缺乏端到端反馈，卡顿率高于主版。
    - 观众端没有默认 1s 延迟，导致弱网下卡顿较大。遇到该问题时请联系我们处理。

**相关链接**

- [Android 迁移指南](https://confluence.agoralab.co/pages/viewpage.action?pageId=681270968)
- [iOS 迁移指南](https://confluence.agoralab.co/display/ADC/iOS)
- [API Sample 示例项目](https://github.com/AgoraIO/API-Examples/tree/2.7.1_fury)
- [内部发版说明](https://confluence.agoralab.co/display/MS/2.7.1+release+note)

## v3.0.225

该版本于 2020 年 7 月 24 日发布。

优化和问题修复如下：
- 优化了压测下 SDK 的稳定性。
- 修复了 iOS 平台上加入频道较慢的问题。

## v3.0.0.23

该版本于 2020 年 7 月 3 日发布。

该版本修复了频道外占用通话音量的问题。

## v3.0.0.22

该版本于 2020 年 6 月 30 日发布。

**新增特性**

#### 自定义音频采集

为方便你自行采集并处理音频数据，该版本新增 `setExternalAudioSource` 方法允许你开启自定义音频源功能并设置外部音频源的参数。你需要在加入频道前调用该方法。处理完音频数据后，你可以通过 `pushExternalAudioFrame` 方法将音视频数据发送回 SDK，以进行后续操作。

#### 原始音频数据

为方便你自行处理音频数据，该版本新增如下接口允许你在编码后和解码前获取并修改原始音频数据：

- `registerAudioFrameObserver`：注册音频观测器。
- `setRecordingAudioFrameParameters`：设置 `onRecordAudioFrame` 报告的音频数据格式。
- `setPlaybackAudioFrameParameters`：设置 `onPlaybackAudioFrame` 报告的音频数据格式。
- `setMixedAudioFrameParameters`：设置 `onMixedAudioFrame` 报告的音频数据格式。
- `setPlaybackAudioFrameBeforeMixingParameters`：设置 `onPlaybackAudioFrameBeforeMixing` 报告的音频数据格式。
- `onRecordAudioFrame`：获取本地用户的原始音频数据。
- `onPlaybackAudioFrame`：获取所有远端用户的原始音频数据。
- `onMixedAudioFrame`：获取本地用户和所有远端用户的原始音频数据。
- `onPlaybackAudioFrameBeforeMixing`：获取特定远端用户的原始音频数据。

你需要在加入频道前调用 `registerAudioFrameObserver` 方法注册音频观测器，并在该方法中实现 `IAudioFrameObserver` 类。注册成功后，SDK 每隔 10 毫秒触发 `onRecordAudioFrame`、`onPlaybackAudioFrame`、`onMixedAudioFrame` 或 `onPlaybackAudioFrameBeforeMixing` 回调。你可以从回调中获取原始音频数据，并根据场景自行处理音频数据。处理完成后，你可以直接播放音频或将音频数据发送回 SDK 以进行后续操作。

<div class="alert note">如果你想修改从回调中获取到的原始音频数据格式，请调用 setRecordingAudioFrameParameters、setPlaybackAudioFrameParameters、setMixedAudioFrameParameters 或 setPlaybackAudioFrameBeforeMixingParameters 方法。</div>

#### 音量管理

该版本新增支持调节本地录音音量和背景音乐的播放音量：

- `adjustRecordingSignalVolume`：调节本地录音音量。
- `adjustAudioMixingPlayoutVolume`：调节音乐文件的本地播放音量。
- `adjustAudioMixingPublishVolume`：调节音乐文件的远端播放音量。

你也可以通过如下方法获取背景音乐在本地和远端的音量：

- `getAudioMixingPlayoutVolume`：获取音乐文件的本地播放音量。
- `getAudioMixingPublishVolume`：获取音乐文件的远端播放音量。

**问题修复**

该版本修复了用户离开频道后，SDK 音频模块未关闭的问题。

## v3.0.0.21

该版本于 2020 年 6 月 5 日发布。主要改进和修复问题如下：

- 优化了 SDK 在 iOS 平台上的 CPU 占用率。对比上一个版本，该版本 CPU 占用减少了 19%。
- 新增支持 Android x64 架构。
- 修复了特定场景下音频路由无法从外放切换到听筒的问题。


## v3.0.0.20

该版本于 2020 年 5 月 8 日发布。

该版本主要针对语聊房场景，提供完整的音频管理方法。具体涵盖的功能及对应 API 详见下文。

**实现功能**

该版本实现了如下功能，且使用逻辑与官网 SDK 完全一致。

#### 频道管理

初始化 RtcEngine 后，你可以通过调用 joinChannel 方法创建并加入频道；通话结束后，调用 leaveChannel 可以离开频道。 在整个 RtcEngine 生命周期内，你都可以通过 getConnectionState 方法获取当前的频道连接状态。

#### 本地发布/远端订阅音频控制

在互动过程中，该版本支持通过如下方法对音频进行更为精细的控制：

- muteLocalAudioStream：停止/继续发送本地音频流
- muteRemoteAudioStream：停止/继续接收指定的远端音频流
- muteAllRemoteAudioStreams：停止/继续接收所有远端音频流

#### 音频状态管理

提供如下回调，方便用户在音频异常时，通过这些回调了解当前的音频状态，及产生异常的原因：

- onFirstLocalAudioFramePublished: 本地音频首帧成功发布回调
- onLocalAudioStateChanged：本地音频状态发生改变回调
- onRemoteAudioStateChanged：远端音频状态发生改变回调

#### 音量检测

为方便了解频道中谁在说话，以及说话人的音量，该版本提供 enableAudioVolumeIndication 方法。通过该方法开启音量检测后，SDK 会在 onAudioVolumeIndication 回调中报告当前频道中音量最高的几个用户，及其音量信息。

#### 音频质量统计

提供 onLocalAudioStats 和 onRemoteAudioStats 回调。在成功加入频道后，SDK 会每两秒触发一次这两个回调，实时报告当前的统计数据。

#### 网络质量统计

提供 onRtcStats 和 onNetworkQuality，方便实时了解当前的网络质量。

#### 音频路由

音频路由是指 app 在播放音频时使用的设备通道。该版本提供如下方法设置音频路由：

- setDefaultAudioRoutetoSpeakerphone：设置默认的音频路由
- setEnableSpeakerphone：暂态启用或禁用扬声器

#### 变声及混响

在社交娱乐应用中，为增加产品的趣味性和互动性，用户常常需要变声和混响效果。该版本通过 setLocalVoiceChanger 和 setLocalVoiceReverbPreset 方法，提供预置的 18 种变声和 16 种混响效果选项。

#### 耳返

通过如下方法，支持耳返功能：

- enableInEarMonitoring：启用耳返功能。该版本还提供一个重载的方法，含有 includeAudioFilter 参数，你可以通过该参数设置耳返中是否应用混音音效。
- setInEarMonitoringVolume：设置耳返音量

#### 音乐文件播放及混响

在实时音视频互动中，用户除了自己说话的声音，有时候需要播放自定义的声音让频道内的其他人也听到。比如连麦合唱场景中，需要播放背景音乐。该版本通过如下方法，提供完整的音乐文件播放与混音功能，帮助你实现更为丰富的场景：

- startAudioMixing：开始播放音乐文件
- stopAudioMixing：停止播放音乐文件
- pauseAudioMixing：暂停播放音乐文件
- resumeAudioMixing：恢复播放音乐文件
- adjustAudioMixingVolume：调节音乐文件播放音量
- getAudioMixingDuration：获取音乐文件的播放进度
- getAudioMixingCurrentPosition：获取音乐文件的播放进度
- setAudioMixingPosition：设置音乐文件的播放位置

在音乐文件播放与混音的过程中，你都可以从 onAudioMixingStateChanged 回调中了解当前音乐播放的状态。

#### 通话前网络测试

在加入频道或切换角色前，进行网络质量探测，可以判断或预测用户当前的网络状况是否良好。

该版本通过 startLastmileProbeTest 方法，支持在加入频道前，对 last-mile 网络质量进行探测。成功启用网络探测后，SDK 会触发 onLastmileProbeResult 回调，报告当前网络质量。

**改进**

除上述功能外，该版本还引进了一些新的方法，或对原有方法的逻辑进行了优化。

#### 频道媒体选项（新增类）

为方便开发者对频道进行更精细的控制，该版本提供了一个重载的 joinChannel 方法，其中包含一个 ChannelMediaOptions 类。在加入频道前，你可以使用该类进行如下默认设置：

- 频道场景
- 本地用户的角色类型
- 发布哪个音、视频轨道<sup>1</sup>
- 是否自动订阅远端流

加入频道以后，你还可以通过 `updateChannelMediaOptions` 方法，动态调整 `ChannelMediaOptions` 类中的设置。

<div class="note alert"><sup>1</sup>：一路流中可以包含多个音频轨道。通过将各音频轨道发布选项设为 true，用户可以同时发布外部采集的 PCM 和麦克风采集的音频轨道。</div>

#### 多频道（新增类）

为方便用户同时加入多个频道，该版本通过 `RtcEngineEx` 和 `IRtcEngineEventHandlerEx` 类，并通过 Connection ID 来标记频道。

你可以调用 `RtcEngineEx` 类中的 `joinChannelEx` 方法，然后设置其中的 `Connection` 参数来实现加入多个频道。加入多个频道后，用户可以同时发布多路视频流和音频流。

#### mute 逻辑（行为优化）

针对老引擎中 `mute` 和 `muteAll` 方法“双锁”逻辑带来的集成问题，该版本对这两个方法的实现进行了优化。

在该版本中，`muteAll` 和 `mute` 方法互不依赖。无论当前设置如何，`muteAll` 都会对当前所有用户，和即将加入频道的用户（如有）生效；`mute` 都会对指定用户生效。

#### 音频路由切换原则（行为优化）

由于设备的音频路由会受用户行为和操作系统制约，实际使用中，音频路由可能会和预期不符。针对这个问题，该版本对音频路由切换的原则进行了优化。

#### 其他改进

- 音频 3A 算法。在直播场景下，该版本应用 3A（回声消除、噪声抑制、自动增益） 算法，优化音频体验。
- Android 平台的平均延迟大幅减少 100 ms。
- Android 平台的耳反减少 100 ms。
- Android 平台减少内存。

## v2.6.3.4

该版本于 2020 年 7 月 31 日发布。

新增特性和问题修复如下：
- 新增支持 Windows 平台上耳麦动态插拔。
- 修复了 Windows 和 Android 平台上的一些 crash，提高了 SDK 稳定性。
- 修复了 iOS 和 macOS 平台上 bundle ID 重复问题。
- 修复了 iOS 平台上 MediaPlayer Kit 播放视频模糊问题。
- 修复了 iOS 平台上少量内存泄漏问题。


## v2.6.3.2

该版本于 2020 年 6 月 12 日发布。该版本修复了如下问题：

- 偶现的开始或停止发布流时，麦克风采集的音量为 0 的问题。
- SDK 初始化影响其他 app 音频播放音量的问题。
- 采用临时方案尝试解决麦克风初始化失败的问题。
- 其他偶现的崩溃问题。

## v2.6.3.1

该版本于 2020 年 5 月 13 日发布。

**支持功能** 

- 为对互动场景进行更有针对性的优化，该版本梳理了 Agora  的频道场景：
	- 增加 CHANNEL_PROFILE_COMMUNICATION_1v1 为 1 v 1 场景优化。
	- 默认 CHANNEL_PROFILE_LIVE_BROADCASTING 作为多人视频场景 。
- 优化了 Android 平台多频道相关 API。
- 提供 registerAudioFrameObserver 方法，支持从当前所有频道中获取 PCM 格式的音频数据。
- iOS 版本支持提升到 9.0。

**改进**

- 优化了观众端的出图速度。
- 优化了连麦 PK 场景下，观众快速切换频道的速度。
- 优化了弱网对抗策略，可以增加有效编码码率约 5%。
- 优化了发送 PCM 音频原始数据场景下的的 CPU 消耗。


**问题修复**
- 修复偶现的崩溃，提升了 SDK 整体的稳定性。
- 修复了特定场景下在使用 registerVideoEncodedImageReceiver 接口接受 H264 编码的数据时， SDK 和其他版本的互通问题。
- 修复了在使用 registerVideoEncodedImageReceiver 场景下，进入频道后，无法订阅远端用户的问题。
- 修复了云录制的黑屏问题。
- 修复了 Windows 平台上，没有麦克风情况下初始化失败的问题。
