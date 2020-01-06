# svelte-youtube

Simple [Svelte](https://svelte.dev/) component acting as a thin layer over the [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference)

## Features

- URL playback
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
  containerClass={string}           // defaults -> ''
  options={obj}                     // defaults -> {}
  on:ready={func}                   // defaults -> noop
  on:play={func}                    // defaults -> noop
  on:pause={func}                   // defaults -> noop
  on:end={func}                     // defaults -> noop
  on:error={func}                   // defaults -> noop
  on:stateChange={func}             // defaults -> noop
  on:playbackRateChange={func}      // defaults -> noop
  on:playbackQualityChange={func}   // defaults -> noop
/>
```

For convenience it is also possible to access the PlayerState constants through svelte-youtube:
`YouTube.PlayerState` contains the values that are used by the [YouTube IFrame Player API](https://developers.google.com/youtube/iframe_api_reference#onStateChange).

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
