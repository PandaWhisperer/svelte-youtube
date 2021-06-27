<script>
	import YouTube from '../../src/index.svelte';

	export let videoId;

	function logEvent(name) {
		return (event) => console.log(`Event received: ${name}`, event.detail);
	}
</script>

<main>
	<h1>Svelte-YouTube</h1>
	<h3>Embedding a single video:</h3>
	<YouTube {videoId}
		on:ready={logEvent('ready')}
		on:play={logEvent('play')}
		on:pause={logEvent('pause')}
		on:end={logEvent('end')}
		/>
	
	<h3>Embedding a responsive video component:</h3>
	<p>By default, svelte-youtube components are <strong>not</strong> responsive.</p>
	<p>
		The responsive comonent will take 100% of the container's width, 
		so style your container using responsive units.
	</p>
	<YouTube {videoId} responsive={true}
			on:ready={logEvent('ready')}
			on:play={logEvent('play')}
			on:pause={logEvent('pause')}
			on:end={logEvent('end')}
	/>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
