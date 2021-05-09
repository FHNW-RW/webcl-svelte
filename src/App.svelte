<script>
    import Eye from './Eye.svelte';
</script>

<main>
    <h1>Svelte</h1>
    <p>SVG Eyes build with <a href="https://svelte.dev/">Svelte</a></p>
    <div class="eye-row">
        <Eye class="eye-col"/>
        <Eye class="eye-col"/>
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
