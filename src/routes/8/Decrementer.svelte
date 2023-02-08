<script>
	import { count } from './stores.js';

	function decrement() {
        // update store value using callback
        count.update(n => n - 1);
	}
</script>

<button on:click={decrement}>
	-
</button>