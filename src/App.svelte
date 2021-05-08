<script>
  import Eye from './Eye.svelte';
</script>

<main>
  <h1>Svelte</h1>
  <p>SVG Eyes build with <a href="https://svelte.dev/">Svelte</a></p>

  <!-- Eyes -->
  <!-- TODO: Move eyes in row -->
  <Eye />
  <Eye />
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
</style>
