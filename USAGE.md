# How to Use react-native-youtube-iframe in React Native

## Installation

### Step 1: Install the package

Since the package is on GitHub, install it directly from the repository:

```bash
npm install https://github.com/Ehtishamulhaq2k2/react-native-youtube-iframe.git
```

Or using yarn:

```bash
yarn add https://github.com/Ehtishamulhaq2k2/react-native-youtube-iframe.git
```

### Step 2: Install peer dependencies

The package requires `react-native-webview`. Install it:

```bash
npm install react-native-webview
# or
yarn add react-native-webview
```

For iOS, you may need to run:
```bash
cd ios && pod install
```

## Basic Usage

### Simple Example

```jsx
import React from 'react';
import {View} from 'react-native';
import YoutubeIframe from 'react-native-youtube-iframe';

function App() {
  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <YoutubeIframe
        height={300}
        width={400}
        videoId="dQw4w9WgXcQ"
      />
    </View>
  );
}

export default App;
```

### With Play/Pause Control

```jsx
import React, {useState, useRef} from 'react';
import {View, Button} from 'react-native';
import YoutubeIframe from 'react-native-youtube-iframe';

function App() {
  const [playing, setPlaying] = useState(false);
  const [muted, setMuted] = useState(false);

  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <YoutubeIframe
        height={300}
        width={400}
        videoId="dQw4w9WgXcQ"
        play={playing}
        mute={muted}
        onChangeState={event => {
          console.log('Player state:', event);
        }}
        onReady={() => {
          console.log('Player is ready');
        }}
        onError={error => {
          console.log('Error:', error);
        }}
      />
      <View style={{marginTop: 20, flexDirection: 'row', gap: 10}}>
        <Button
          title={playing ? 'Pause' : 'Play'}
          onPress={() => setPlaying(!playing)}
        />
        <Button
          title={muted ? 'Unmute' : 'Mute'}
          onPress={() => setMuted(!muted)}
        />
      </View>
    </View>
  );
}

export default App;
```

### Using Ref Methods

```jsx
import React, {useRef, useState} from 'react';
import {View, Button, Text} from 'react-native';
import YoutubeIframe from 'react-native-youtube-iframe';

function App() {
  const playerRef = useRef(null);
  const [duration, setDuration] = useState(0);
  const [currentTime, setCurrentTime] = useState(0);

  const getVideoInfo = async () => {
    try {
      const videoDuration = await playerRef.current?.getDuration();
      const time = await playerRef.current?.getCurrentTime();
      setDuration(videoDuration || 0);
      setCurrentTime(time || 0);
    } catch (error) {
      console.error('Error getting video info:', error);
    }
  };

  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <YoutubeIframe
        ref={playerRef}
        height={300}
        width={400}
        videoId="dQw4w9WgXcQ"
        onReady={() => {
          console.log('Player is ready');
        }}
      />
      <View style={{marginTop: 20}}>
        <Button title="Get Video Info" onPress={getVideoInfo} />
        <Text>Duration: {duration}s</Text>
        <Text>Current Time: {currentTime}s</Text>
      </View>
    </View>
  );
}

export default App;
```

### With Playlist

```jsx
import React from 'react';
import {View} from 'react-native';
import YoutubeIframe from 'react-native-youtube-iframe';

function App() {
  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <YoutubeIframe
        height={300}
        width={400}
        playList={['dQw4w9WgXcQ', 'jNQXAC9IVRw']} // Array of video IDs
        playListStartIndex={0}
        play={true}
      />
    </View>
  );
}

export default App;
```

### Using Player States and Errors

```jsx
import React, {useState} from 'react';
import {View, Text} from 'react-native';
import YoutubeIframe, {PLAYER_STATES, PLAYER_ERRORS} from 'react-native-youtube-iframe';

function App() {
  const [playerState, setPlayerState] = useState(PLAYER_STATES.UNSTARTED);

  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      <YoutubeIframe
        height={300}
        width={400}
        videoId="dQw4w9WgXcQ"
        onChangeState={event => {
          setPlayerState(event);
          console.log('State changed to:', event);
        }}
        onError={error => {
          console.log('Error:', error);
          if (error === PLAYER_ERRORS.VIDEO_NOT_FOUND) {
            console.log('Video not found!');
          }
        }}
      />
      <Text style={{marginTop: 20}}>
        Player State: {playerState}
      </Text>
    </View>
  );
}

export default App;
```

### Fetching Video Metadata

```jsx
import React, {useEffect, useState} from 'react';
import {View, Text} from 'react-native';
import {getYoutubeMeta} from 'react-native-youtube-iframe';

function App() {
  const [videoMeta, setVideoMeta] = useState(null);

  useEffect(() => {
    getYoutubeMeta('dQw4w9WgXcQ')
      .then(meta => {
        setVideoMeta(meta);
        console.log('Video Title:', meta.title);
        console.log('Author:', meta.author_name);
      })
      .catch(error => {
        console.error('Error fetching metadata:', error);
      });
  }, []);

  return (
    <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
      {videoMeta && (
        <View>
          <Text>Title: {videoMeta.title}</Text>
          <Text>Author: {videoMeta.author_name}</Text>
          <Text>Duration: {videoMeta.height}x{videoMeta.width}</Text>
        </View>
      )}
    </View>
  );
}

export default App;
```

## Available Props

### Required Props
- `height` (number): Height of the webview container (minimum 200px)
- `width` (number, optional): Width of the webview container (minimum 200px)

### Video Props
- `videoId` (string, optional): YouTube Video ID
- `playList` (string | Array<string>, optional): Playlist ID or array of video IDs
- `play` (boolean, default: false): Play/pause control
- `mute` (boolean, default: false): Mute/unmute control
- `volume` (number, default: 100): Volume level (0-100)
- `playbackRate` (number, default: 1): Playback speed
- `playListStartIndex` (number, default: 0): Start index for playlist

### Callback Props
- `onReady` (function): Called when player is ready
- `onError` (function): Called when an error occurs
- `onChangeState` (function): Called when player state changes
- `onFullScreenChange` (function): Called when fullscreen state changes
- `onPlaybackQualityChange` (function): Called when playback quality changes
- `onPlaybackRateChange` (function): Called when playback rate changes

### Style Props
- `viewContainerStyle` (StyleProp<ViewStyle>): Style for container View
- `webViewStyle` (StyleProp<ViewStyle>): Style for WebView
- `webViewProps` (WebViewProps): Additional props for underlying WebView

### Advanced Props
- `initialPlayerParams` (object): Initial YouTube player parameters
- `allowWebViewZoom` (boolean, default: false): Allow zooming in webview
- `forceAndroidAutoplay` (boolean, default: false): Force autoplay on Android
- `contentScale` (number, default: 1.0): Scale factor for viewport
- `useLocalHTML` (boolean, default: false): Use locally generated HTML
- `baseUrlOverride` (string): Override base URL for iframe

## Ref Methods

When using a ref, you can access these methods:

```typescript
playerRef.current?.getDuration()           // Returns Promise<number>
playerRef.current?.getVideoUrl()           // Returns Promise<string>
playerRef.current?.getCurrentTime()       // Returns Promise<number>
playerRef.current?.isMuted()              // Returns Promise<boolean>
playerRef.current?.getVolume()            // Returns Promise<number>
playerRef.current?.getPlaybackRate()      // Returns Promise<number>
playerRef.current?.getAvailablePlaybackRates() // Returns Promise<number[]>
playerRef.current?.seekTo(seconds, allowSeekAhead) // void
playerRef.current?.playVideo()            // void
playerRef.current?.pauseVideo()            // void
playerRef.current?.stopVideo()             // void
```

## Player States

Available player states (from `PLAYER_STATES`):
- `UNSTARTED`
- `ENDED`
- `PLAYING`
- `PAUSED`
- `BUFFERING`
- `VIDEO_CUED`

## Player Errors

Available error types (from `PLAYER_ERRORS`):
- `HTML5_ERROR`
- `VIDEO_NOT_FOUND`
- `EMBED_NOT_ALLOWED`
- `INVALID_PARAMETER`

## Notes

- Minimum viewport size: 200px x 200px
- Recommended size for 16:9 players: 480px x 270px minimum
- The package automatically builds on installation via the `prepare` script
- Works on both iOS and Android
- Supports Expo projects

