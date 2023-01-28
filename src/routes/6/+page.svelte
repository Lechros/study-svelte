<script>
    let name = 'world';

	let a = 1;
	let b = 2;
</script>

<input bind:value={name}>

<h1>Hello {name}!</h1>

<!--
    svelte takes care of converting types
    - input with type=number/range will set numeric value
    - input with type=text will set string value
    - two different input type might change variable type
-->
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