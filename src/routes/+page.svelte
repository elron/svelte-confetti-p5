<script lang="ts">
  import ConfettiP5 from "../lib/ConfettiP5.svelte";
  import { expirable } from "@macfja/svelte-expirable";
  import { cat1, cat2 } from "./cats";

  export const statuses = expirable();

  let fallenParticles = 0;

  const onEachFall = () => fallenParticles++;
  const onStart = () => statuses.push(["on:start", new Date()], 2);
  const onDestroy = () => statuses.push(["on:destroy", new Date()], 2);
  const onDone = () => statuses.push(["on:done", new Date()], 2);

  const styles = [
    "Default",
    "blue slow loop",
    "child balls",
    "fire squares",
    "snow flakes",
    "ice cubes",
    "cat images",
  ] as const;
  type Style = (typeof styles)[number];

  const bgs = [
    "white",
    "black",
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

  $: {
    currentStyle;
    replay;
    forcedDestroy = false;
  }

  let forcedDestroy = false;
</script>

<div class="wrapper" style:background={currentBg} class:offset>
  <h1>Svelte Confetti P5</h1>
  <div class="option">
    <h3>Status:</h3>
    <button on:click={() => replay++}>Replay</button>
    <button on:click={() => (forcedDestroy = true)}>Destroy</button>
    {#each [...$statuses].reverse() as status, i}
      {status.data[0]}
      <!-- {#key i}
        <span out:slide|global={{ delay: 3000 }}>
          <span out:fade|global in:scale|global={{ start: 0.5 }}
            >{status.data[0]}</span
          >
        </span>
      {/key} -->
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
    {#if !forcedDestroy}
      {#if currentStyle === "Default"}
        <ConfettiP5
          on:eachfall={onEachFall}
          on:start={onStart}
          on:done={onDone}
          on:destory={onDestroy}
        />
      {/if}
      {#if currentStyle === "blue slow loop"}
        <ConfettiP5
          on:eachfall={onEachFall}
          on:start={onStart}
          on:done={onDone}
          on:destory={onDestroy}
          loop
          colors={["#fbfefd", "#00186a", "#003afb", "#7aa8f7"]}
          amount={120}
          minSize={5}
          maxSize={15}
          spacing={15}
          flip={3}
          wind={0}
          weight={100}
          rotate={3}
          power={0}
        />
      {/if}
      {#if currentStyle === "child balls"}
        <ConfettiP5
          on:eachfall={onEachFall}
          on:start={onStart}
          on:done={onDone}
          on:destory={onDestroy}
          amount={40}
          weight={300}
          shapes="circles"
          minSize={35}
          maxSize={50}
          flip={0}
          power={25}
          spacing={10}
          rotate={3}
        />
      {/if}
      {#if currentStyle === "fire squares"}
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
      {#if currentStyle === "snow flakes"}
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
      {#if currentStyle === "ice cubes"}
        <ConfettiP5
          on:eachfall={onEachFall}
          on:start={onStart}
          on:done={onDone}
          on:destory={onDestroy}
          colors={["#e7edf3", "#e4f3ff", "#b9e1ff", "#a6d9ff"]}
          shapes="squares"
          minSize={20}
          maxSize={40}
          spacing={30}
          amount={30}
          rotate={0.2}
          flip={0}
          wind={0}
          power={-10}
          weight={500}
        />
      {/if}
      {#if currentStyle === "cat images"}
        <ConfettiP5
          on:eachfall={onEachFall}
          on:start={onStart}
          on:done={onDone}
          on:destory={onDestroy}
          shapes="images"
          flip={0}
          amount={100}
          spacing={30}
          power={0}
          rotate={0.2}
          weight={110}
          wind={15}
          images={[cat2,cat1]}
        />
      {/if}
    {/if}
  {/key}
  <footer>
    <a
      href="https://github.com/elron/svelte-confetti-p5/blob/master/src/routes/%2Bpage.svelte"
      target="_blank">View source code on GitHub</a
    >
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
  a {
    color: rgb(73, 109, 217);
  }
  :global(.p5Canvas) {
    // This fixes some viability issue in confettipage iframe
    visibility: visible !important;
  }
</style>
