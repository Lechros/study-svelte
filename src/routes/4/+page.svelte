<script>
	let user = { loggedIn: false };

    function toggle() {
        user.loggedIn = !user.loggedIn;
    }

    let x = 7;
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