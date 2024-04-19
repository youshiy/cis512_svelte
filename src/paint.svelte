<script>
  import { onMount } from 'svelte';
  let canvas, ctx;
  let isDrawing = false;
  let lineWidth = 5;
  let strokeColor = '#000000';
  let isErasing = false;

  onMount(() => {
    ctx = canvas.getContext('2d');
    ctx.lineJoin = 'round';
    ctx.lineCap = 'round';
  });

  function handleMouseDown(event) {
    isDrawing = true;
    ctx.beginPath();
   
    ctx.moveTo(event.offsetX, event.offsetY);
  }

  function handleMouseMove(event) {
    if (!isDrawing) return;
    ctx.lineWidth = lineWidth;
    ctx.strokeStyle = isErasing ? 'white' : strokeColor;
    ctx.lineTo(event.offsetX, event.offsetY);
    ctx.stroke();
  }

  function handleMouseUpOrOut() {
    isDrawing = false;
  }

  function clearCanvas() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
  }
</script>

<style>
  canvas {
    border: 1px solid black;
    background-color: white;
  }
  button {
    background-color: pink; 
    border: 1px solid #ccc; 
    padding: 0.5em 1em; 
    cursor: pointer; 
  }

  button:active {
    background-color: purple; 
  }
</style>

<div>
  <input type="color" bind:value={strokeColor} />
  <input type="range" min="1" max="50" bind:value={lineWidth} />
  <button on:click={() => (isErasing = !isErasing)}>
    {isErasing ? 'Paint Mode' : 'Erase Mode'}
  </button>
  <button on:click={clearCanvas}>Clear Canvas</button>
</div>

<canvas id="canvas-element"
        bind:this={canvas}
        width="800"
        height="800"
        on:mousedown={handleMouseDown}
        on:mousemove={handleMouseMove}
        on:mouseup={handleMouseUpOrOut}
        on:mouseout={handleMouseUpOrOut}>
</canvas>
