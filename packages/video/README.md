# @nativescript/video

Basic video player for NativeScript iOS and Android.

## Installation

```javascript
npm install @nativescript/video
```

## What does this plugin do?

This is a more basic video player plugin with less access to additional Android features as it uses Android MediaPlayer. For a more advanced video player plugin, use [@nativescript/video-advanced](https://github.com/NativeScript/plugins/tree/main/packages/video-advanced).

This was originally based off of nStudio's [nativescript-videoplayer](https://github.com/nstudio/nativescript-videoplayer).

The plugin uses the MediaPlayer on Android and AVPlayer on iOS to play local and remote videos.

#### Native platform controls:

| Android                                                                                       | iOS                                                                                                                               |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Android MediaPlayer](https://developer.android.com/reference/android/media/MediaPlayer.html) | [iOS AVPlayer](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayer_Class/index.html) |

| Android Sample                             | iOS Sample                                   |
| ------------------------------------------ | -------------------------------------------- |
| ![Android Sample](./screenshots/video.gif) | ![iOS Sample](./screenshots/videoplayer.gif) |

## Usage

```XML
<Page xmlns="http://schemas.nativescript.org/tns.xsd"
      xmlns:VideoPlayer="@nativescript/video">
        <StackLayout>
            <VideoPlayer:Video id="nativeVideoPlayer"
            controls="true" loop="true" autoplay="false" height="280"
            src="~/videos/big_buck_bunny.mp4" />
        </StackLayout>
</Page>
```

```typescript
import { Video } from '@nativescript/video';

const video = topmost().currentPage.getViewById('nativeVideoPlayer') as Video;
// Setting event listeners on the Video
video.on(Video.pausedEvent, () => {
	console.log('Video has been paused.');
});

video.on(Video.mutedEvent, () => {
	console.log('Video has been muted.');
});

// changing the src
video.src = 'some video file or url';

// set loop
video.loop = false;
```

### Angular NativeScript Usage

```TS
// somewhere at top of your component or bootstrap file
import { registerElement } from '@nativescript/angular';
import { Video } from '@nativescript/video';
registerElement('VideoPlayer', () => Video);
// documentation: https://v7.docs.nativescript.org/angular/plugins/angular-third-party.html
```

_When using Angular you have to explicitly close all components so the correct template code is below._

```XML
  <VideoPlayer
      src="http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4"
      autoplay="true"
      height="300"></VideoPlayer>
```

## Properties

| Property                            | Description                                                                                                                                                                                                                             |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **src**                             | The src file for the video. Set the video file to play, for best performance use local video files if possible. The file must adhere to the platforms accepted video formats. For reference check the platform specs on playing videos. |
| **autoplay - (boolean)**            | Set if the video should start playing as soon as possible or to wait for user interaction.                                                                                                                                              |
| **controls - (boolean)**            | Set to use the native video player's media playback controls.                                                                                                                                                                           |
| **muted - (boolean)**               | Mutes the native video player.                                                                                                                                                                                                          |
| **loop - (boolean)**                | Sets the native video player to loop once playback has finished.                                                                                                                                                                        |
| **fill - (boolean)**                | If true, the aspect ratio of the video will not be honored and it will fill the entire space available.                                                                                                                                 |
| **observeCurrentTime - (boolean)**  | If true, currentTimeUpdated callback is possible.                                                                                                                                                                                       |
| **headers - (Map<string, string>)** | Set headers to add when loading a video from URL.                                                                                                                                                                                       |

## API

| Method                       | Platform | Description                                                         |
| ---------------------------- | -------- | ------------------------------------------------------------------- |
| **play()**                   | both     | Start playing the video                                             |
| **pause()**                  | both     | Pause the video                                                     |
| **stop()**                   | android  | Stop the playback - this resets the player and remove the video src |
| **seekToTime(time: number)** | both     | Seek the video to a time (milliseconds)                             |
| **getCurrentTime()**         | both     | Returns the current time in the video duration (milliseconds)       |
| **getDuration()**            | both     | Returns the duration of the video (milliseconds)                    |
| **destroy()**                | both     | Destroy the video player and free resources                         |
| **mute(boolean)**            | both     | Mute the current video                                              |
| **setVolume()**              | both     | Set the volume - Must be between 0 and 1                            |

## Observable Properties

- currentTime() - Current time of video.

## Events

| Event                   | Description                                                         |
| ----------------------- | ------------------------------------------------------------------- |
| errorEvent              | This event fires when an error in the source code is thrown.        |
| playbackReadyEvent      | This event fires when the video is ready.                           |
| playbackStartEvent      | This event fires when video starts playback.                        |
| seekToTimeCompleteEvent | This event fires when seeking is complete.                          |
| currentTimeUpdatedEvent | This event fires when the current time of playing video is changed. |
| finishedEvent           | This event fires when the video is complete.                        |
| mutedEvent              | This event fires when video is muted.                               |
| unmutedEvent            | This event fires when video is unmutedEvent.                        |
| pausedEvent             | This event fires when video is paused.                              |
| volumeSetEvent          | This event fires when the volume is set.                            |

## iOS Logging

When running the iOS Simulator, after playing a video the iOS system may write
log messages to the console every few seconds of the form

```
[aqme] 254: AQDefaultDevice (173): skipping input stream 0 0 0x0
```

They will continue being logged even after the video has been properly destroyed.
These messages can be safely ignored. To turn them off completely, run the following
command in your shell before running `ns run ios`:

```
export SIMCTL_CHILD_OS_ACTIVITY_MODE="disable"
```

## License

Apache License Version 2.0
