<script context="module">
  /**
   * Expose PlayerState constants for convenience. These constants can also be
   * accessed through the global YT object after the YouTube IFrame API is instantiated.
   * https://developers.google.com/youtube/iframe_api_reference#onStateChange
   */
  export const PlayerState = {
    UNSTARTED: -1,
    ENDED: 0,
    PLAYING: 1,
    PAUSED: 2,
    BUFFERING: 3,
    CUED: 5,
  };
</script>

<script>
  import { onMount } from 'svelte';
  import { createEventDispatcher } from 'svelte';
  import YoutubePlayer from 'youtube-player';

  export let id = undefined; // HTML element ID for player
  export let url;            // Youtube video URL

  let playerElem;            // player DOM element reference
  let player;                // player API instance

  // Create and tear down player as component mounts or unmounts
  onMount(() => createPlayer());

  // Update videoId and load new video if URL changes
  $: play(url);

  function createPlayer() {
    player = YoutubePlayer(playerElem);

    // Register event handlers
    player.on('ready', onPlayerReady);
    player.on('error', onPlayerError);
    player.on('stateChange', onPlayerStateChange);
    player.on('playbackRateChange', onPlayerPlaybackRateChange);
    player.on('playbackQualityChange', onPlayerPlaybackQualityChange);

    // Tear down player when done
    return () => player.destroy();
  }

  function play(url) {
    if (player && url) {
      const videoId = getYoutubeId(url);

      player.loadVideoById(videoId);
    }
  }

  // -------------------------------------------
  // Event handling
  // -------------------------------------------
	const dispatch = createEventDispatcher();

  /**
   * https://developers.google.com/youtube/iframe_api_reference#onReady
   *
   * @param {Object} event
   *   @param {Object} target - player object
   */
  function onPlayerReady(event) {
    dispatch('ready', event);

    // Start playing
    play(url);
  }

  /**
   * https://developers.google.com/youtube/iframe_api_reference#onError
   *
   * @param {Object} event
   *   @param {Integer} data  - error type
   *   @param {Object} target - player object
   */
  function onPlayerError(event) {
    dispatch('error', event);
  }

  /**
   * https://developers.google.com/youtube/iframe_api_reference#onStateChange
   *
   * @param {Object} event
   *   @param {Integer} data  - status change type
   *   @param {Object} target - actual YT player
   */
  function onPlayerStateChange(event) {
    dispatch('stateChange', event)

    switch (event.data) {
      case PlayerState.ENDED:
        dispatch('end', event);
        break;

      case PlayerState.PLAYING:
        dispatch('play', event);
        break;

      case PlayerState.PAUSED:
        timer.clear();
        dispatch('pause', event);
        break;

      default:
    }
  }

  /**
   * https://developers.google.com/youtube/iframe_api_reference#onPlaybackRateChange
   *
   * @param {Object} event
   *   @param {Float} data    - playback rate
   *   @param {Object} target - actual YT player
   */
  function onPlayerPlaybackRateChange(event) {
    dispatch('playbackRateChange', event);
  }

  /**
   * https://developers.google.com/youtube/iframe_api_reference#onPlaybackQualityChange
   *
   * @param {Object} event
   *   @param {String} data   - playback quality
   *   @param {Object} target - actual YT player
   */
  function onPlayerPlaybackQualityChange(event) {
    dispatch('playbackQualityChange', event);
  }
</script>

<div class="youtube-player">
  <div id={id} bind:this={playerElem}></div>
</div>
