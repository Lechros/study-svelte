<script>
	import { set_custom_element_data } from "svelte/internal";


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

    let numbers = [1, 2, 3, 4];

    function addNumber() {
        // this doesn't trigger update...
        numbers.push(numbers.length + 1);
        // need to assign to `numbers`
        numbers = numbers;
    }

    // this will trigger update since left side is reactive variable
    // numbers[numbers.length] = numbers.length + 1;

    $: sum = numbers.reduce((t, n) => t + n, 0);

    let deep = {
        first: {
            second: {
                arr: [1, 2, 3],
                hello: 'world',
            },
            another: {
                arr: [0, 9, 8]
            }
        }
    };

    function silentAdd() {
        deep.first.second.arr.push(deep.first.second.arr.length);
        deep.first.another.arr.push(10 - deep.first.another.arr.length);
    }

    function changeHello() {
        deep.first.second.hello = deep.first.second.hello + "!";
    }
</script>

<button on:click={incrementCount}>
    Clicked {count} {count === 1 ? 'time' : 'times'}
</button>

<p>{count} doubled is {doubled}</p>

<p>{numbers.join(' + ')} = {sum}</p>

<button on:click={addNumber}>Add a number</button>

<!--
    reactivity test

    assigning to `deep.first.second.hello` triggers update
    on every variable that is desendent of `deep`
-->

<p>{deep.first.second.arr}</p>
<p>{deep.first.another.arr}</p>

<p>{deep.first.second.hello}</p>

<button on:click={silentAdd}>Silently add a number</button>

<button on:click={changeHello}>Add ! to hello</button>
