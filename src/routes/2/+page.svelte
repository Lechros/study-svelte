<script>
    let count = 0;
    // reactive declarations: auto updated when other variabled change
    $: doubled = count * 2;

    function incrementCount() {
        // reactive on assignment
        count += 1;
    }
</script>

<button on:click={incrementCount}>
    Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>