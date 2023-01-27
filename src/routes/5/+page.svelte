<script>
	import { prevent_default } from "svelte/internal";


    let m = { x: 0, y: 0 };

    function handleMousemove(event) {
        m.x = event.clientX;
        m.y = event.clientY;

    }

    function handleClick() {
        alert('no more alerts');
    }

    import Inner from './Inner.svelte';

    function handleMessage(event) {
        alert(event.detail.text);
    }

    import Outer from "./Outer.svelte";
</script>

<!-- inline event handler: free syntax highlighting, no performance hit -->
<div on:mousemove={e => m = { x: e.clientX, y: e.clientY }}>
    The mouse position is {m.x} x {m.y}
</div>

<!--
    handler modifiers

    preventDefault: calls `event.preventDefault()`
    stopPropagation: calls `event.stopPropagation()`
    passive: improve scroll performance (auto added)
    nonpassive: opposite of passive
    capture: run during the capture phase instead of the bubbling phase
    once: remove handler after first run
    self: only trigger if event.target is element itself
    trusted: trigger only when `event.isTrusted` is true

    chaining is allowed
-->
<button on:click|once={handleClick}>
    Click me
</button>

<!-- custom event name 'message' -->
<Inner on:message={handleMessage}/>

<Outer on:message={handleMessage}/>

<style>
    div {
        width: 100%;
        height: 100%;
        background-color: aquamarine;
    }
</style>