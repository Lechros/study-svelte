<script>
	import { count } from './stores.js';
	import Incrementer from './Incrementer.svelte';
	import Decrementer from './Decrementer.svelte';
	import Resetter from './Resetter.svelte';
	// import { onDestroy } from 'svelte';

	// // unsubscribing gets boilerplatey
	// let countValue;

	// const unsubscribe = count.subscribe(value => {
	// 	countValue = value;
	// });

	// onDestroy(unsubscribe);

	// this logs only once, not reactive
	console.log($count);
	// use `$:` label to 'subscribe'
	$: console.log($count); // == count.subscribe(value => console.log(value))
</script>

<!--
	prefix store variable with `$` to reference value
-->
<h1>The count is {$count}</h1>

<Incrementer/>
<Decrementer/>
<Resetter/>