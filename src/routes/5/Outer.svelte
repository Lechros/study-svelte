<script>
    import Inner from "./Inner.svelte";
</script>

<!-- svelte component events doesn't bubble -->
<!-- below forwards events (same as on:message={ dispatch('message', event.detail) })-->
<Inner on:message/>