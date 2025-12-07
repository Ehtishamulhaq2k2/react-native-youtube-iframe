# React Native Youtube iframe

![npm](https://img.shields.io/npm/v/react-native-youtube-iframe?style=for-the-badge) ![npm](https://img.shields.io/npm/dm/react-native-youtube-iframe?style=for-the-badge)

A wrapper of the Youtube IFrame player API build for react native.

- ✅ Works seamlessly on both ios and android platforms
- ✅ Does not rely on the native youtube service on android (prevents unexpected crashes, works on phones without the youtube app)
- ✅ Uses the webview player which is known to be more stable compared to the native youtube app
- ✅ Access to a vast API provided through the iframe youtube API
- ✅ Supports multiple youtube player instances in a single page
- ✅ Fetch basic video metadata without API keys (uses oEmbed)
- ✅ Works on modals and overlay components
- ✅ Expo support

![ios](./website/static/img/demo.gif?raw=true 'ios')

## Installation

### Install from GitHub

```bash
npm install https://github.com/Ehtishamulhaq2k2/react-native-youtube-iframe.git
```

Or using yarn:

```bash
yarn add https://github.com/Ehtishamulhaq2k2/react-native-youtube-iframe.git
```

### Install Peer Dependencies

The package requires `react-native-webview`:

```bash
npm install react-native-webview
# or
yarn add react-native-webview
```

For iOS, run pod install:

```bash
cd ios && pod install
```

## Quick Start

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

## Documentation

For detailed usage examples and API documentation, see [USAGE.md](./USAGE.md)

Original documentation: [react-native-youtube-iframe](https://lonelycpp.github.io/react-native-youtube-iframe/)

## Contributing

Pull requests are welcome!

Read the [contributing guide](./CONTRIBUTING.md) for project setup and other info

## License

[MIT](https://choosealicense.com/licenses/mit/)
