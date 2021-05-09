<script>
  import { writable } from 'svelte/store';

  let lidsOpen = false;

  let iris;
  let irisEllipse;
  let closeLidLayer;
  let openLidLayer;
  export let mouseX;
  export let mouseY;

  let mouseCoords = writable({ x: 60, y: 60 });

  export function closeOrOpenLids() {
    closeLidLayer.style.opacity = lidsOpen ? '1' : '0';
    lidsOpen = !lidsOpen;
  }

  function moveIris(e) {
    //rect should be size of svg
    let svg = document.getElementById("eye")
    let rect = svg.getBoundingClientRect()
    // TODO: center eyes to center
    // TODO: limit eyes to not go outside. See: https://github.com/WebEngineering-FHNW/webcl-fs21-2/blob/main/week7/svg-eyes-sketch/Eyes.html
    // TODO: Is there an easier way with Svelte? Bindings, Motion? Check https://svelte.dev/tutorial/spring
    // TODO: Set intervall for blinking eyes
    const xo = rect.x + rect.width/2;
    console.log(rect)
    const yo = rect.y + rect.height/2;
    const xm = e.clientX - xo;
    const ym = e.clientY - yo;

    const xmax = rect.width/1.5;
    const ymax = rect.height/2;

    const widestFocus = 400; // when x is so far away, the eye is maximal extended

    const scaledX = xm * (xmax / widestFocus );
    let   xe = xm > 0
            ? Math.min( xmax, scaledX)
            : Math.max(-xmax, scaledX);
    const scaledY = ym * (ymax / widestFocus );
    let   ye = ym > 0
            ? Math.min( ymax, scaledY)
            : Math.max(-ymax, scaledY);
    if (xe*xe + ye*ye > xmax * ymax) {
      xe *= 0.9;
      ye *= 0.9;
    }

    mouseCoords.set({ x: xe, y: ye });
  }
</script>

<svg id="eye" viewBox="0 0 120 120" on:mousemove={moveIris} on:click={closeOrOpenLids}>
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
    <ellipse bind:this={irisEllipse} cx={$mouseCoords.x} cy={$mouseCoords.y} rx="30" ry="30" opacity="1" fill="url(#gradient1)" />
    <ellipse cx={$mouseCoords.x} cy={$mouseCoords.y} rx="7" ry="7" opacity="1" fill="#FFFFFF" fill-opacity="0.8" />
  </g>
  <g bind:this={openLidLayer}>
    <path d="M0 60 A60,60 0 0,1 120,60 A60,30 0 0,0 0,60 Z" opacity="1" fill="#FDDC99" fill-opacity="1" filter="url(#shadow)" />
    <path d="M0 60 A60,60 0 0,0 120,60 A60,40 0 0,1 0,60 Z" opacity="1" fill="#F4CB76" fill-opacity="1" />
  </g>
  <g bind:this={closeLidLayer}>
    <path d="M0 60 A60,60 0 0,1 120,60 A60,40 0 0,1 0,60 Z" opacity="1" fill="#FDDC99" fill-opacity="1" />
  </g>
</svg>