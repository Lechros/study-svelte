<script>
    let name = 'svelte';
    let src = '/tutorial/image.gif';
</script>

<h1>Hello {name.toUpperCase()}!</h1>

<img src={src} alt='{name} dances.'>
<!-- shorthand attributes: name and value are the same -->
<img {src} alt='{name} dances.'>