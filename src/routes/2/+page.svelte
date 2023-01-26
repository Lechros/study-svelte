<script>
    let count = 0;
    // reactive declarations: auto updated when other variabled change
    $: doubled = count * 2;
    // reactive statements: run arbitrary statements reactively
    $: console.log('the count is ' + count);
    // group
    $: {
        // console.log(...);
        // alert(...);
    }
    // if (this doesn't run without reactivity label)
    $: if(count >= 10) {
        alert('count is dangerously high!');
        count = 9;
    }

    function incrementCount() {
        // reactive on assignment
        count += 1;
    }
</script>

<button on:click={incrementCount}>
    Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>