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

	let html = '<p>Write some text!</p>';

	let todos = [
		{ done: false, text: 'finish Svelte tutorial' },
		{ done: false, text: 'build an app' },
		{ done: false, text: 'world domination' }
	];

	function add() {
		todos = todos.concat({ done: false, text: '' });
	}

	function clear() {
		todos = todos.filter(t => !t.done);
	}

	$: remaining = todos.filter(t => !t.done).length;

	let time;
	let duration;
	let paused;

	let w, h;

	import { onMount } from 'svelte';

	// undefined until component is mounted
	let canvas;

	onMount(() => {
		const ctx = canvas.getContext('2d');
		let frame = requestAnimationFrame(loop);

		function loop(t) {
			frame = requestAnimationFrame(loop);

			const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);

			for (let p = 0; p < imageData.data.length; p += 4) {
				const i = p / 4;
				const x = i % canvas.width;
				const y = i / canvas.width >>> 0;

				const r = 64 + (128 * x / canvas.width) + (64 * Math.sin(t / 1000));
				const g = 64 + (128 * y / canvas.height) + (64 * Math.cos(t / 1000));
				const b = 128;

				imageData.data[p + 0] = r;
				imageData.data[p + 1] = g;
				imageData.data[p + 2] = b;
				imageData.data[p + 3] = 255;
			}

			ctx.putImageData(imageData, 0, 0);
		}

		return () => {
			cancelAnimationFrame(frame);
		};
	});

	import Keypad from './Keypad.svelte';

	let pin;
	$: view = pin ? pin.replace(/\d(?!$)/g, '???') : 'enter your pin';

	function handleSubmit3() {
		alert(`submitted ${pin}`);
	}

	import InputField from './InputField.svelte';
	let field;
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

<div contenteditable="true" bind:innerHTML={html}></div>

<pre>{html}</pre>

<h1>Todos</h1>

{#each todos as todo}
	<div class:done={todo.done}>
		<input
			type=checkbox
			bind:checked={todo.done}
		>

		<input
			placeholder="What needs to be done?"
			bind:value={todo.text}
		>
	</div>
{/each}

<p>{remaining} remaining</p>

<button on:click={add}>
	Add new
</button>

<button on:click={clear}>
	Clear completed
</button>

<video>
	bind:currentTime={time}
	bind:duration
	bind:paused
	<track kind="captions">
</video>

<br>
<br>
<br>

<p>Size: {w}px x {h}px</p>

<!--
	clientWidth/clientHeight is readonly
	`display: inline` elements and elements that can't contain other elements
	cannot be measured with this property (measure wrapper instead)
-->
<div bind:clientWidth={w} bind:clientHeight={h}>
	<span style="font-size: 42px">edit me</span>
</div>


<!-- bind:this will set element reference (use where react ref)-->
<canvas
	bind:this={canvas}
	width={32}
	height={32}
></canvas>

<h1 style="color: {pin ? '#333' : '#ccc'}">{view}</h1>

<!-- bind to component exported variable -->
<Keypad bind:value={pin} on:submit={handleSubmit3}/>

<InputField bind:this={field} />

<button on:click="{() => field.focus()}">
	Focus field
</button>

<style>
	label { display: flex }
	input, p { margin: 6px }
	textarea { width: 100%; height: 200px; }
	input {
		display: block;
		width: 500px;
		max-width: 100%;
	}
	[contenteditable] {
		padding: 0.5em;
		border: 1px solid #eee;
		border-radius: 4px;
	}
	.done {
		opacity: 0.4;
	}
</style>