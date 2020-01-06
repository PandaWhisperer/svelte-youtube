# svelte-youtube

Simple [Svelte](https://svelte.dev/) component acting as a thin layer over the [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference). Based on [react-youtube] (https://github.com/tjallingt/react-youtube).

## Features

- URL playback (soon)
- [playback event bindings](https://developers.google.com/youtube/iframe_api_reference#Events)
- [customizable player options](https://developers.google.com/youtube/player_parameters)

## Installation

```
$ npm install svelte-youtube
```

## Usage

```js
<script>
  import YouTube from 'svelte-youtube';
</script>

<YouTube
  videoId={string}                  // defaults -> null
  id={string}                       // defaults -> null
  class={string}                    // defaults -> null
  options={obj}                     // defaults -> {}
/>
```

## Events

The following events are available:

-  `on:ready`: Player has finished loading and is ready to play
-  `on:play`: Playback has started
-  `on:pause`: Playback has been paused
-  `on:end`: Playback has ended
-  `on:error`: An error has occurred (see below)
-  `on:stateChange`: Player State has changed (see below)
-  `on:playbackRateChange`: Playback rate has changed (see below)
-  `on:playbackQualityChange`: Playback quality has changed (see below)

Each event's `detail` property contains a `data` and a `target` property (except for the `ready` event, which does not have a `data` property). The `target` is a reference to the player instance, while the `data` contains information specific to the event.

For details on the contents of the `data` property, and for a more detailed description of each event, refer to the [YouTube IFrame Player API Events](https://developers.google.com/youtube/iframe_api_reference#Events) .

### Player State

For convenience it is also possible to access the PlayerState constants through svelte-youtube.

The `PlayerState` named export contains the values that are used by the [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference#onStateChange).

### Player Errors

Refer to [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference#onError) for an explanation of the error codes used in the `on:error` event.

## Example

```js
<script>
  import YouTube from 'svelte-youtube';

  const options = {
    height: '390',
    width: '640',
    //  see https://developers.google.com/youtube/player_parameters
    playerVars: {
      autoplay: 1
    }
  };

  function onReady(event) {
    // access to player in all event handlers via event.target
    event.target.pauseVideo();
  }
</script>

<YouTube videoId="2g811Eo7K8U" {options} on:ready={onReady} />
```

## Controlling the player

You can access & control the player in a way similar to the [official api](https://developers.google.com/youtube/iframe_api_reference#Events):

> The ~~API~~ *component* will pass an event object as the sole argument to each of ~~those functions~~ *the event handler props*. The event object has the following properties:

> * The event's `target` identifies the video player that corresponds to the event.
> * The event's `data` specifies a value relevant to the event. Note that the `onReady` event does not specify a `data` property.

## License

MIT
