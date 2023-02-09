<script>
	import { value } from "./store";
</script>

<a href="./other">other</a>

<h1>The value is: {$value}</h1>
<button on:click={() => $value += 1}>increment</button>