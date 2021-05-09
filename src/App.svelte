<script>
    import Eye from './Eye.svelte';
    let wh = window.innerHeight;
    let ww = window.innerWidth;

    // init it to something random if mouse starts outside window
    let mx = Math.random() * ww;
    let my = Math.random() * wh;

    function onmousemove(event) {
        mouseX = event.pageX - rect.x
        mouseY = event.pageY - rect.y
    }


</script>

<main>
    <h1>Svelte</h1>
    <p>SVG Eyes build with <a href="https://svelte.dev/">Svelte</a></p>
    <div id=class="eye-row" on:mousemove={onmousemove}>
        <Eye class="eye-col" {mouseX} {mouseY} />
        <Eye class="eye-col" {mouseX} {mouseY} />
    </div>
</main>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 240px;
        margin: 0 auto;
    }

    h1 {
        color: #ff3e00;
        text-transform: uppercase;
        font-size: 4em;
        font-weight: 100;
    }

    .eye-row {
        display: flex;
    }

    .eye-col {
        flex: 1;
        padding: 1em;
    }
</style>
