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

	import Eliza from 'elizabot';
	import { beforeUpdate, afterUpdate } from 'svelte';

	let div;

	beforeUpdate(() => {
		// determine whether we should auto-scroll
		// once the DOM is updated...
	});

	afterUpdate(() => {
		// ...the DOM is now in sync with the data
	});

	const eliza = new Eliza();

	let comments = [
		{ author: 'eliza', text: eliza.getInitial() }
	];

	function handleKeydown(event) {
		if (event.key === 'Enter') {
			const text = event.target.value;
			if (!text) return;

			comments = comments.concat({
				author: 'user',
				text
			});

			event.target.value = '';

			const reply = eliza.transform(text);

			setTimeout(() => {
				comments = comments.concat({
					author: 'eliza',
					text: '...',
					placeholder: true
				});

				setTimeout(() => {
					comments = comments.filter(comment => !comment.placeholder).concat({
						author: 'eliza',
						text: reply
					});
				}, 500 + Math.random() * 500);
			}, 200 + Math.random() * 200);
		}
	}

	let div2;
	let autoscroll;

	beforeUpdate(() => {
		autoscroll = div2 && (div2.offsetHeight + div2.scrollTop) > (div2.scrollHeight - 20);
	});

	afterUpdate(() => {
		if (autoscroll) div2.scrollTo(0, div2.scrollHeight);
	});

	import { tick } from 'svelte';

	let text = `Select some text and hit the tab key to toggle uppercase`;

	async function handleKeydown2(event) {
		if (event.key !== 'Tab') return;

		event.preventDefault();

		const { selectionStart, selectionEnd, value } = this;
		const selection = value.slice(selectionStart, selectionEnd);

		const replacement = /[a-z]/.test(selection)
			? selection.toUpperCase()
			: selection.toLowerCase();

		text = (
			value.slice(0, selectionStart) +
			replacement +
			value.slice(selectionEnd)
		);

		// selection is updated after svelte update
		await tick();
		this.selectionStart = selectionStart;
		this.selectionEnd = selectionEnd;
	}
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

<div class="chat">
	<h1>Eliza</h1>

	<div class="scrollable" bind:this={div2}>
		{#each comments as comment}
			<article class={comment.author}>
				<span>{comment.text}</span>
			</article>
		{/each}
	</div>

	<input on:keydown={handleKeydown}>
</div>

<textarea value={text} on:keydown={handleKeydown2}></textarea>

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

	.chat {
		display: flex;
		flex-direction: column;
		height: 100%;
		max-width: 320px;
	}

	.scrollable {
		flex: 1 1 auto;
		border-top: 1px solid #eee;
		margin: 0 0 0.5em 0;
		overflow-y: auto;
	}

	article {
		margin: 0.5em 0;
	}

	.user {
		text-align: right;
	}

	span {
		padding: 0.5em 1em;
		display: inline-block;
	}

	.eliza span {
		background-color: #eee;
		border-radius: 1em 1em 1em 0;
	}

	.user span {
		background-color: #0074D9;
		color: white;
		border-radius: 1em 1em 0 1em;
		word-break: break-all;
	}

	textarea {
		width: 100%;
		height: 200px;
	}
</style>