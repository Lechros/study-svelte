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