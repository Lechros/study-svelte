<script>
	import { count } from './stores.js';

	function reset() {
        // set store value
        count.set(0);
	}
</script>

<button on:click={reset}>
	reset
</button>