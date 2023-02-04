<script>
  import { onMount } from "svelte";
  import { cubicInOut } from "svelte/easing";
  import { tweened } from "svelte/motion";

  const initial = {
    start: [60, 400],
    start_control: [6, 4],
    end_control: [600, 400],
    end: [500, 20],
  };

  const final = {
    start: [60, 400],
    start_control: [420, 200],
    end_control: [30, 50],
    end: [500, 20],
  };

  const curve = tweened(initial, {
    duration: 1000,
    easing: cubicInOut,
  });

  async function animate() {
    curve.set(initial, { duration: 0 });
    curve.set(final);
  }

  let show_controls = true;

  onMount(animate);
</script>

<h1>svg path tweened animation</h1>

<svg width="800" height="800" fill="none">
  <path
    d="
	 	M{$curve.start[0]},{$curve.start[1]}
	 	C{$curve.start_control[0]},{$curve.start_control[1]}
		{$curve.end_control[0]},{$curve.end_control[1]}
		{$curve.end[0]},{$curve.end[1]}"
    stroke="#CCFF00"
    stroke-width="27"
    stroke-linecap="round"
  />
  {#if show_controls}
    <line
      x1={$curve.start[0]}
      y1={$curve.start[1]}
      x2={$curve.start_control[0]}
      y2={$curve.start_control[1]}
      stroke-width="2"
      stroke="#00000040"
    />
    <line
      x1={$curve.end[0]}
      y1={$curve.end[1]}
      x2={$curve.end_control[0]}
      y2={$curve.end_control[1]}
      stroke-width="2"
      stroke="#00000040"
    />
  {/if}
</svg>

<label>
  <input type="checkbox" bind:checked={show_controls} />
  show_controls
</label>

<button on:click={animate}> rerun </button>

<style>
  svg,
  label,
  button {
    display: block;
  }
  button {
    font-size: 20px;
  }
  svg {
    border-radius: 5px;
    outline: 2px solid black;
  }
</style>
