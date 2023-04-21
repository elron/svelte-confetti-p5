<script lang="ts">
  import { fade, slide } from "svelte/transition";
  import ConfettiP5 from "../lib/ConfettiP5.svelte";
  import { expirable } from "@macfja/svelte-expirable";

  export const statuses = expirable();

  let fallenParticles = 0;

  const onEachFall = () => fallenParticles++;
  const onStart = () => statuses.push("on:start", 1);
  const onDestroy = () => statuses.push("on:destroy", 1);
  const onDone = () => statuses.push("on:done", 1);

  const styles = ["Default", 1, 2, 3, 4, 5, 6, 7] as const;
  type Style = (typeof styles)[number];

  const bgs = [
    "black",
    "white",
    "gray",
    "tomato",
    "deepskyblue",
    "hotpink",
    "indigo",
  ] as const;
  type Bg = (typeof bgs)[number];

  let currentStyle: Style = "Default";
  let currentBg: Bg = "white";

  const offsetColors = ["black", "indigo"];
  $: offset = !!offsetColors.includes(currentBg);

  let replay = 0;
</script>

<div class="wrapper" style:background={currentBg} class:offset>
  <h1>Svelte Confetti P5</h1>
  <div class="option">
    <h3>Status:</h3>
    <button on:click={() => replay++}>Replay</button>
    {#each $statuses as status}
      <span in:slide={{ axis: "x" }}>{status.data}</span>
    {/each}
  </div>
  <!-- <div class="option">Particles Fallen: {fallenParticles}</div> -->
  <div class="option">
    <h3>Background:</h3>
    {#each bgs as bg}
      <button
        style:background={bg}
        style:color={bg === "black" ? "white" : null}
        class:current={bg === currentBg}
        class:offset={offsetColors.includes(bg)}
        on:click={() => (currentBg = bg)}>{bg}</button
      >
    {/each}
  </div>
  <div class="option">
    <h3>Example Styles:</h3>
    {#each styles as style}
      <button
        on:click={() => {
          currentStyle = style;
          replay++;
        }}>{style}</button
      >
    {/each}
  </div>
  <div class="option">
    <h3>Confetties Fallen:</h3>
    <button on:click={() => (fallenParticles = 0)}>Reset</button>
    {fallenParticles}
  </div>

  {#key replay}
    {#if currentStyle === "Default"}
      <ConfettiP5
        on:eachfall={onEachFall}
        on:start={onStart}
        on:done={onDone}
        on:destory={onDestroy}
      />
    {/if}
    {#if currentStyle === 1}
      <ConfettiP5
        on:eachfall={onEachFall}
        on:start={onStart}
        on:done={onDone}
        on:destory={onDestroy}
        loop
        colors={["#fbfefd", "#00186a", "#003afb", "#7aa8f7"]}
        amount={150}
        weight={100}
        minSize={5}
        maxSize={15}
        spacing={10}
        flip={3}
        rotate={3}
        power={10}
      />
    {/if}
    {#if currentStyle === 2}
      <ConfettiP5
        on:eachfall={onEachFall}
        on:start={onStart}
        on:done={onDone}
        on:destory={onDestroy}
        colors={["#fbfefd", "#00186a", "#003afb", "#7aa8f7"]}
        amount={50}
        weight={200}
        minSize={20}
        maxSize={50}
        spacing={3}
        flip={3}
        rotate={3}
        power={20}
      />
    {/if}
    {#if currentStyle === 3}
      <ConfettiP5
        on:eachfall={onEachFall}
        on:start={onStart}
        on:done={onDone}
        on:destory={onDestroy}
        colors={["#e84b5e", "#f9db05", "#fefffc", "#ffbe24"]}
        shapes="squares"
        minSize={5}
        maxSize={25}
        spacing={15}
        amount={80}
        flip={2}
        power={5}
        weight={200}
      />
    {/if}
    {#if currentStyle === 4}
      <ConfettiP5
        on:eachfall={onEachFall}
        on:start={onStart}
        on:done={onDone}
        on:destory={onDestroy}
        colors={["#e7edf3", "#e4f3ff", "#b9e1ff", "#a6d9ff"]}
        shapes="circles"
        minSize={0}
        maxSize={14}
        spacing={5}
        amount={50}
        flip={0}
        wind={0}
        power={2}
        weight={50}
      />
    {/if}
  {/key}
  <footer>
    <a href="">View source code</a>
  </footer>
</div>


<style lang="scss">
  :global(body) {
    margin: 0;
    font-family: Arial, Helvetica, sans-serif;
  }
  .wrapper {
    padding: 3em;
    min-height: 100vh;
    &.offset {
      color: white;
    }
  }
  .option {
    margin-bottom: 2em;
    h3 {
      margin: 0 0 0.5em;
    }
    span {
      display: inline-flex;
      margin-inline-end: 0.3em;
    }
  }
  button {
    margin-inline-end: 0.3em;
    border: 0;
    background: #ddd;
    &.current {
      // box-shadow: 0 0 0 1px white inset;
    }
    box-shadow: 0 1.5px 3px 0px rgba(0, 0, 0, 0.2);
    color: black;
    &.offset {
      color: white;
      // &.current {
      //   box-shadow: 0 0 0 1px black inset;
      // }
    }
    padding: 0.5em 0.8em;
    font-weight: bold;
    border-radius: 0.5em;
  }
</style>
