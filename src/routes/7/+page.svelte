<script>
    import { onMount, onDestroy } from 'svelte';

	let photos = [];

    // fetch inside onMount because of SSR.
    // lifecycle functions don't run during SSR
	onMount(async () => {
		const res = await fetch(`/tutorial/api/album`);
		photos = await res.json();
	});

	import Timer from './Timer.svelte';

    let open = true;
    let seconds = 0;

    const toggle = () => (open = !open);
    const handleTick = () => (seconds += 1);
</script>

<h1>Photo album</h1>

<div class="photos">
	{#each photos as photo}
		<figure>
			<img src={photo.thumbnailUrl} alt={photo.title}>
			<figcaption>{photo.title}</figcaption>
		</figure>
	{:else}
		<!-- this block renders when photos.length === 0 -->
		<p>loading...</p>
	{/each}
</div>

<div>
	<button on:click={toggle}>{open ? 'Close' : 'Open'} Timer</button>
	<p>
		The Timer component has been open for
		{seconds} {seconds === 1 ? 'second' : 'seconds'}
	</p>
	{#if open}
	<Timer callback={handleTick} />
	{/if}
</div>

<style>
	.photos {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure, img {
		width: 100%;
		margin: 0;
	}
</style>