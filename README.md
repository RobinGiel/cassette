# react-responsive-audio-player

![react-responsive-audio-player in action](demo.gif)

### [see a live demo here](https://benwiley4000.github.io/react-responsive-audio-player/)

## the replaceable responsive audio player

React Component wrapper for the HTML audio tag. Out of the box, provides a featured, clean-looking audio player which works great on desktop and mobile. Offers a powerful API for building a rich, totally customized audio player UI.

[![NPM](https://nodei.co/npm/react-responsive-audio-player.png)](https://npmjs.org/package/react-responsive-audio-player)

**If you're not using npm and you need production-ready scripts to include in your project, check out [the releases](https://github.com/benwiley4000/react-responsive-audio-player/releases).**

## Usage
HTML:
```html
<!DOCTYPE html>
<html>
  <head><link rel="stylesheet" href="audioplayer.css"></head>
  <body>
    <div id="audio_player_container"></div>
    <script src="dist/main.js"></script>
  </body>
</html>
```
JavaScript (with JSX):
```javascript
// dist/main.js
var React = require('react');
var ReactDOM = require('react-dom');
var AudioPlayer = require('react-responsive-audio-player');

var playlist =
  [{ url: 'audio/track1.mp3',
     title: 'Track 1 by Some Artist' },
   { url: 'audio/track2.mp3',
     title: 'Some Other Artist - Track 2' }];
ReactDOM.render(
  <AudioPlayer playlist={playlist} />,
  document.getElementById('audio_player_container')
);
```
JavaScript (without JSX):
```javascript
// dist/main.js
...
ReactDOM.render(
  React.createElement(AudioPlayer, {
    playlist: playlist
  }),
  document.getElementById('audio_player_container')
);
```

## Getting started
### Quick start
The fastest way to get off the ground with this module is to paste the following code into an HTML file and open it in a web browser:
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>React Responsive Audio Player</title>
    <style> html, body { margin: 0; background: lightseagreen; } </style>
    <link rel="stylesheet" href="https://unpkg.com/react-responsive-audio-player@1.3.1/dist/audioplayer.css">
  </head>
  <body>
    <div id="audio_player_container"></div>

    <script src="https://unpkg.com/react@16.3.0-alpha.0/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@16.3.0-alpha.0/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/prop-types/prop-types.js"></script>
    <script src="https://unpkg.com/resize-observer-polyfill"></script>
    <script src="https://unpkg.com/react-responsive-audio-player@1.3.1/dist/audioplayer.js"></script>
    <script>
      var playlist =
        [{ url: 'song1.mp3', title: 'Track 1 - a track to remember' },
         { url: 'song2.ogg', title: 'Oggs Oggs Oggs' }];
      ReactDOM.render(
        React.createElement(AudioPlayer, {
          playlist: playlist,
          autoplay: true,
          autoplayDelayInSeconds: 2.1,
          style: { position: 'fixed', bottom: 0 },
          controls: ['playpause', 'forwardskip', 'progressdisplay']
        }),
        document.getElementById('audio_player_container')
      );
    </script>
  </body>
</html>
```
Of course you'll need to include paths to actual audio files, or the player will display and not work.

### Package installation
If you use [npm](https://www.npmjs.com/) and a front-end package bundling system like [Browserify](http://browserify.org/) or [webpack](https://webpack.github.io/), it's recommended that you install the package and its dependencies in your project:
```
npm install --save react-responsive-audio-player react react-dom
```
While `react-dom` isn't technically a peer dependency, you'll need it if you plan to place the audio player in the DOM, which you probably will.

When you first include the component in your project it might not look how you're expecting. Be sure to check the **Styling** section below.

### Pre-built releases
If you prefer not to use a package bundler, you can find built releases to download [here](https://github.com/benwiley4000/react-responsive-audio-player/releases).

## Options
Options can be passed to the AudioPlayer element as props. Currently supported props are:

* `playlist` (*required*): an array containing data about the tracks which will be played. **undefined** by default. Each track object can contain the following properties:
  - `url` (*required*): Can be one of:
    - A string containing the address of the audio file to play
    - An array of objects, if you want to specify multiple files of different types for the same track. Each object requires the properties:
      - `src` (*required*): A string containing the address of a file that can be played for this track
      - `type` (*required*): A string which is the [audio file's MIME type](https://developer.mozilla.org/en-US/docs/Web/HTML/Supported_media_formats)
  - `title`: The title of the track - corresponds to the [`MediaMetadata.title` property](https://wicg.github.io/mediasession/#examples)
  - `artist`: The track's artist - corresponds to the [`MediaMetadata.artist` property](https://wicg.github.io/mediasession/#examples)
  - `album`: The album the track belongs to - corresponds to the [`MediaMetadata.album` property](https://wicg.github.io/mediasession/#examples)
  - `artwork`: The artwork for the track - corresponds to the [`MediaMetadata.artwork` property](https://wicg.github.io/mediasession/#examples)
  - `meta`: An object containing any other track-specific information you want to store

* `controls`: an array of keyword strings which correspond to available audio control components. The order of keywords translates to the order of rendered controls. The default array is: `['spacer', 'backskip', 'playpause', 'forwardskip', 'spacer', 'progress']`. The possible keyword values are:
  - `'playpause'` (play/pause toggle button)
  - `'backskip'` (previous track skip button)
  - `'forwardskip'` (next track skip button)
  - `'volume'` (a control for adjusting volume and toggling mute)
  - `'repeat'` (a control which cycles between no-repeat, repeat-playlist, repeat-track)
  - `'shuffle'` (a control which toggles a shuffle mode)
  - `'progress'` (a drag-to-seek audio progress bar)
  - `'progressdisplay'` (a read-only [non-draggable] progress bar)
  - `'spacer'` (a transparent space-filling element whose default width is `10px`, although [the style of the `.spacer` class can be overridden](src/styles/_Spacer.scss))

* `autoplay`: a boolean value (`true`/`false`) that if true will cause the player to begin automatically once mounted. **false** by default.

* `autoplayDelayInSeconds`: a number value that represents the number of seconds to wait until beginning autoplay. Will be ignored if `autoplay` is false. **0** by default. *NOTE:* Delay is managed by `setTimeout` and is therefore inexact. If you need to time an autoplay exactly, find a different module that uses the [WebAudio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API) for playback (or fork this one!).

* `gapLengthInSeconds`: a number value that represents the number of seconds to wait at the end of a track before beginning the next one in the playlist. Not applicable for manually-initiated skip. **0** by default. *NOTE:* Like `autoplayDelayInSeconds`, this delay is inexact.

* `cycle`: a boolean value that if true continues playing from the beginning after the playlist has completed. **true** by default.

* `loadFirstTrackOnPlaylistComplete`: a boolean value that if true loads up the first track when the playlist has completed. Ignored if `cycle` is true.

* `pauseOnSeekPreview`: a boolean value that if true pauses audio while the user is selecting new timestamp for playback. true by default.

* `maintainPlaybackRate`: stops playback rate from changing on `src` update. **false** by default.

* `stayOnBackSkipThreshold`: a number value that represents the number of seconds of progress after which pressing the back button will simply restart the current track. **5** by default.

* `supportedMediaSessionActions`: an array of [Media Session API action names](https://wicg.github.io/mediasession/#actions-model). This determines which system audio controls should be available on platforms supporting the Media Session API. It is *not* the same as the `controls` array. The default array is: `['play', 'pause', 'previoustrack', 'nexttrack']`. The possible values are:
  - `'play'` (ignored at present since systems should provide a default implementation regardless)
  - `'pause'` (ignored at present, for same reason as `'play'`)
  - `'seekbackward'`
  - `'seekforward'`
  - `'previoustrack'`
  - `'nexttrack'`

* `mediaSessionSeekLengthInSeconds`: a number value representing the number of seconds forward or backward to seek when handling the Media Session API `seekbackward` and `seekforward` actions. **10** by default.

* `getDisplayText`: a function which takes a track object and returns a string used to represent that track in the UI. By default, the track will be displayed as "[artist] - [title]".

* `style`: a React style object which is applied to the outermost div in the component. **undefined** by default.

* `onMediaEvent`: An object where the keys are [media event types](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Media_events) and the values are callback functions. **undefined** by default.

* `audioElementRef`: A callback function called after the component mounts and before it unmounts. Similar to [React ref callback prop](https://facebook.github.io/react/docs/refs-and-the-dom.html#the-ref-callback-attribute) but its only parameter is the internally-referenced HTML audio element, not the component itself. **undefined** by default. *NOTE:* This ref should not be used for audio element event listeners; use `onMediaEvent`.

None of these options are required, though the player will be functionally disabled if no `playlist` prop is provided.

## Styling

In order to use the default stylings you'll need to grab the compiled `audioplayer.css` sheet from the module's `dist/` directory. Again, if you're not using npm, you can get the sheet [here](https://github.com/benwiley4000/react-responsive-audio-player/releases).

It's easy to override the default styles with CSS. Alternatively, for styles which only affect the outer element, you can use [React inline styles](https://facebook.github.io/react/docs/dom-elements.html#style).

For example, if you want your audio player to take the full screen width, do the following:
* Include the following code in your own CSS:
  ```css
  html,
  body {
    margin: 0;
  }
  ```
* Give your audio player fixed position styling, e.g.
  ```jsx
  <AudioPlayer style={{ position: 'fixed', bottom: 0 }} />
  ```

## Usage with SASS

If you preprocess your styles with Sass, you can have more powerful control via Sass variables. The defaults are located at the top of [**src/index.scss**](src/index.scss):

```scss
$audio_player_base_height: 50px !default;
$audio_player_base_bg_color: #333 !default;
$audio_player_base_text_color: #fff !default;
// ...etc
```
The `!default` flag means you can override these variable definitions before the styles are included.

```scss
// Include var overrides before default styles import
$audio_player_base_bg_color: firebrick;

// Using webpack css/sass module import syntax
@import '~react-responsive-audio-player/src/index';

// include other overrides afterward
.audio_player {
  width: 600px;
}
```

# Development

For building and testing instructions, see [CONTRIBUTING.md](CONTRIBUTING.md).

# Icons

The standard control components make use of icons from various sources.

The CSS YouTube-style play/pause button and the skip button were authored in part by [@benwiley4000](https://github.com/benwiley4000), with heavy assistance from [this CodePen](https://codepen.io/aralon/pen/NqGWXZ%20) by [@aralon](https://github.com/Aralon).

The CSS volume icon set is from [icono](https://github.com/saeedalipoor/icono) by [@saeedalipoor](https://github.com/saeedalipoor).

The SVG repeat icons and shuffle icon come from Google's [material-design-icons](https://github.com/google/material-design-icons).
