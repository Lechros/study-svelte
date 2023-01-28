<script>
    let name = 'world';

	let a = 1;
	let b = 2;

	let yes = false;

    let scoops = 1;
	let flavours = ['Mint choc chip'];

	function join(flavours) {
		if (flavours.length === 1) return flavours[0];
		return `${flavours.slice(0, -1).join(', ')} and ${flavours[flavours.length - 1]}`;
	}

    let menu = [
        'Cookies and cream',
        'Mint choc chip',
        'Raspberry ripple'
    ];

	import { marked } from 'marked';
	let value = `Some words are *italic*, some are **bold**`;

	let questions = [
		{ id: 1, text: `Where did you go to school?` },
		{ id: 2, text: `What is your mother's name?` },
		{ id: 3, text: `What is another personal fact that an attacker could easily find with Google?` }
	];

	let selected;

	let answer = '';

	function handleSubmit() {
		alert(`answered question ${selected.id} (${selected.text}) with "${answer}"`);
	}
</script>

<!--
    svelte takes care of converting types
    - input with type=number/range will set numeric value
    - input with type=text will set string value
    - input with type=checkbox will set boolean `checked` value
    - input with type=radio with bind:group will set value to input's `value`
    - input with type=checkbox with bind:group will set value to array of input's `value`
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

<h2>Size</h2>

<label>
	<input type=radio bind:group={scoops} name="scoops" value={1}>
	One scoop
</label>

<label>
	<input type=radio bind:group={scoops} name="scoops" value={2}>
	Two scoops
</label>

<label>
	<input type=radio bind:group={scoops} name="scoops" value={3}>
	Three scoops
</label>

<h2>Flavours</h2>

<select multiple bind:value={flavours}>
	{#each menu as flavour}
		<option value={flavour}>
			{flavour}
		</option>
	{/each}
</select>

{#each menu as flavour}
    <label>
		<input type=checkbox bind:group={flavours} name="flavours" value={flavour}>
        {flavour}
    </label>
{/each}

{#if flavours.length === 0}
	<p>Please select at least one flavour</p>
{:else if flavours.length > scoops}
	<p>Can't order more flavours than scoops!</p>
{:else}
	<p>
		You ordered {scoops} {scoops === 1 ? 'scoop' : 'scoops'}
		of {join(flavours)}
	</p>
{/if}

<!-- marked: markdown parser -->
{@html marked(value)}

<textarea bind:value={value}></textarea>

<h2>Insecurity questions</h2>

<form on:submit|preventDefault={handleSubmit}>
	<!--
		select will set value to first in list
		only after binding is initialized
		before that value is `undefined`
	-->
	<select bind:value={selected} on:change="{() => answer = ''}">
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>

	<input bind:value={answer}>

	<button disabled={!answer} type=submit>
		Submit
	</button>
</form>

<p>selected question {selected ? selected.id : '[waiting...]'}</p>

<style>
	label { display: flex }
	input, p { margin: 6px }
	textarea { width: 100%; height: 200px; }
	input {
		display: block;
		width: 500px;
		max-width: 100%;
	}
</style>