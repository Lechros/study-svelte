<script>
    let name = 'svelte';
    let src = '/tutorial/image.gif';
    import Nested from './Nested.svelte';
    let string = 'this string contains some <strong>HTML!!!</strong>';
</script>

<h1>Hello {name.toUpperCase()}!</h1>

<img src={src} alt='{name} dances.'>
<!-- shorthand attributes: name and value are the same -->
<img {src} alt='{name} dances.'>

<p>This is a paragraph.</p>
<Nested/>

<!-- string as html: sanitize before use! -->
<p>{@html string}</p>

<!-- styling: scoped to component (.svelte file) -->
<style>
    p {
        color: purple;
        font-family: 'Comic Sans MS', cursive;
        font-size: 2em;
    }
</style>