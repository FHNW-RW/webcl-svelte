<script>
  import { onMount } from 'svelte';

  // when x is so far away, the eye is maximal extended
  const widestFocus = 400;

  let iris;
  let irisEllipse;
  let origin;
  let max;
  let lidsClosed = false;

  // reactivity assignment: https://svelte.dev/tutorial/reactive-assignments
  $: closeLidOpacity = lidsClosed ? '1' : '0';

  onMount(() => {
    const rect = irisEllipse.getBoundingClientRect();

    origin = {
      x: rect.x + rect.width / 2,
      y: rect.y + rect.height / 2,
    };

    max = {
      x: rect.width / 1.5,
      y: rect.height / 2,
    };
  });

  setInterval(() => {
    if (lidsClosed) return; // do not close and then open if already closed
    lidsClosed = true;
    setTimeout(() => (lidsClosed = false), 300);
  }, 3 * 1000);

  export function closeOrOpenLids() {
    lidsClosed = !lidsClosed;
  }

  export function moveIris(evt) {
    // the normalized x/y coords to work with
    const xm = evt.clientX - origin.x;
    const ym = evt.clientY - origin.y;

    const scaledX = xm * (max.x / widestFocus);
    let xe = xm > 0 ? Math.min(max.x, scaledX) : Math.max(-max.x, scaledX);
    const scaledY = ym * (max.y / widestFocus);
    let ye = ym > 0 ? Math.min(max.y, scaledY) : Math.max(-max.y, scaledY);
    if (xe * xe + ye * ye > max.x * max.y) {
      xe *= 0.9;
      ye *= 0.9;
    }
    iris.style.transform = `translateX(${xe}px) translateY(${ye}px)`;
  }
</script>

<div class="eye">
  <svg viewBox="0 0 120 120" on:click={closeOrOpenLids}>
    <filter id="shadow">
      <feGaussianBlur in="SourceAlpha" stdDeviation="3" />
      <feOffset dx="0" dy="8" />
      <feColorMatrix
        type="matrix"
        values="0 0 0 0  0
                                0 0 0 0  0
                                0 0 0 0  0
                                0 0 0 .5 0"
      />
      <feBlend in="SourceGraphic" mode="normal" />
    </filter>
    <radialGradient id="gradient1" gradientUnits="objectBoundingBox" cx="50%" cy="50%" r="50%">
      <stop offset="38%" stop-color="#000000" stop-opacity="1" />
      <stop offset="46%" stop-color="#073F80" stop-opacity="1" />
      <stop offset="90%" stop-color="#8EC0DD" stop-opacity="1" />
      <stop offset="100%" stop-color="#2F3A46" stop-opacity="1" />
    </radialGradient>
    <g bind:this={iris}>
      <ellipse bind:this={irisEllipse} cx="60" cy="60" rx="30" ry="30" opacity="1" fill="url(#gradient1)" />
      <ellipse cx="50" cy="50" rx="7" ry="7" opacity="1" fill="#FFFFFF" fill-opacity="0.8" />
    </g>
    <g>
      <path d="M0 60 A60,60 0 0,1 120,60 A60,30 0 0,0 0,60 Z" opacity="1" fill="#FDDC99" fill-opacity="1" filter="url(#shadow)" />
      <path d="M0 60 A60,60 0 0,0 120,60 A60,40 0 0,1 0,60 Z" opacity="1" fill="#F4CB76" fill-opacity="1" />
    </g>
    <g>
      <path d="M0 60 A60,60 0 0,1 120,60 A60,40 0 0,1 0,60 Z" opacity={closeLidOpacity} fill="#FDDC99" fill-opacity="1" />
    </g>
  </svg>
</div>

<style>
  .eye {
    display: inline-block;
    width: 60px;
    height: 60px;
  }
</style>
