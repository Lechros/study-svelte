<script>
	let user = { loggedIn: false };

    function toggle() {
        user.loggedIn = !user.loggedIn;
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