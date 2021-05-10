<script>
  import Eye from './Eye.svelte';

  let rightEye;
  let leftEye;

  function handleMousemove(event) {
    rightEye.moveIris(event);
    leftEye.moveIris(event);
  }
</script>

<!-- events: https://svelte.dev/tutorial/dom-events -->
<main on:mousemove={handleMousemove}>
  <h1>Svelte</h1>
  <p>SVG Eyes build with <a href="https://svelte.dev/">Svelte</a></p>

  <!-- this binding: https://svelte.dev/tutorial/bind-this -->
  <Eye bind:this={leftEye} />
  <Eye bind:this={rightEye} />
</main>

<style>
  main {
    text-align: center;
    width: 100%;
    height: 100%;
    margin: 0 auto;
  }

  h1 {
    color: #ff3e00;
    text-transform: uppercase;
    font-size: 4em;
    font-weight: 100;
  }
</style>
