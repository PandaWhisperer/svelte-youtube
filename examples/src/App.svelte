<script>
	import YouTube from '../../src/index.svelte';

	export let videoId;
</script>

<main>
	<h1>Svelte-YouTube</h1>
	<p>Embedding a single video:</p>
	<YouTube {videoId} />
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
