<script>
    let name = 'world';

	let a = 1;
	let b = 2;

	let yes = false;
</script>

<!--
    svelte takes care of converting types
    - input with type=number/range will set numeric value
    - input with type=text will set string value
    - input with type=checkbox will set boolean `checked` value
    - two different input type might change variable type
-->

<input bind:value={name}>

<h1>Hello {name}!</h1>
<label>
	<input type=number bind:value={a} min=0 max=10>
	<input type=range bind:value={a} min=0 max=10>
</label>

<label>
	<input type=number bind:value={b} min=0 max=10>
	<input type=range bind:value={b} min=0 max=10>
</label>

<p>{a} + {b} = {a + b}</p>

<style>
	label { display: flex }
	input, p { margin: 6px }
</style>

<label>
	<input type=checkbox bind:checked={yes}>
	Yes! Send me regular email spam
</label>

{#if yes}
	<p>Thank you. We will bombard your inbox and sell your personal details.</p>
{:else}
	<p>You must opt-in to continue. If you're not paying, you're the product.</p>
{/if}

<button disabled={!yes}>
	Subscribe
</button>