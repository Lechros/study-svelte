<script>
	let user = { loggedIn: false };

    function toggle() {
        user.loggedIn = !user.loggedIn;
    }

    let x = 7;

    let cats = [
		{ id: 'J---aiyznGQ', name: 'Keyboard Cat' },
		{ id: 'z_AbfPXTKms', name: 'Maru' },
		{ id: 'OUtn3pvWmpg', name: 'Henri The Existential Cat' }
	];

    import Thing from './Thing.svelte';

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' },
	];

	function handleClick() {
		things = things.slice(1);
	}

    async function getRandomNumber() {
		const res = await fetch(`/tutorial/random-number`);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	let promise = getRandomNumber();

	function handleClick2() {
		promise = getRandomNumber();
	}
</script>
<!-- conditionally render markup (still adds indentation level T^T) -->
<!--
    # block start tag
    : block continuation tag
    / block end tag
-->
{#if user.loggedIn}
    <button on:click={toggle}>
        Log out
    </button>
<!-- continue with else block -->
{:else}
    <button on:click={toggle}>
        Log in
    </button>
{/if}

{#if x > 10}
<p>{x} is greater than 10</p>
{:else if 5 > x}
<p>{x} is less than 5</p>
{:else}
<p>{x} is between 5 and 10</p>
{/if}

<h1>The Famous Cats of YouTube</h1>

<ul>
    <!-- each blocks -->
    {#each cats as cat, index}
        <li><a target="_blank" href="https://www.youtube.com/watch?v={cat.id}" rel="noreferrer">
            {cat.name}
        </a></li>
    {/each}
    <!-- destructuring -->
    {#each cats as {id, name}}
        <li><a target="_blank" href="https://www.youtube.com/watch?v={id}" rel="noreferrer">
            {name}
        </a></li>
    {/each}
</ul>

<button on:click={handleClick}>
	Remove first thing
</button>

<!-- specify key for blocks (key goes in (...)) -->
<!-- similar to react but svelte sets default key (maybe index?) instead of showing console error -->
<!-- maybe svelte try to reuse components for performance? -->
{#each things as thing (thing.id)}
	<Thing name={thing.name}/>
{/each}

<button on:click={handleClick2}>
	generate random number
</button>

{#await promise}
    <p>{promise}</p>
{:then number}
    <p>The number is {number}</p>
{:catch error}
    <p style="color: red">{error.message}</p>
{/await}