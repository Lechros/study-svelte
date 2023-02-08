<script>
	import { count } from './stores.js';

	function increment() {
        // update store value using callback
        count.update(n => n + 1);
	}
</script>

<button on:click={increment}>
	+
</button>