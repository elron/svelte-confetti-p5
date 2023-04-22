[![Netlify Status](https://api.netlify.com/api/v1/badges/12a4d8fb-c15c-4766-8d1d-7d0d00217505/deploy-status)](https://app.netlify.com/sites/svelte-confetti-p5/deploys)
![npm](https://img.shields.io/npm/dw/svelte-confetti-p5)
![npm](https://img.shields.io/npm/v/svelte-confetti-p5)

[DEMO & Examples](https://svelte-confetti-p5.netlify.app/) | [REPL](https://svelte.dev/repl/f68f6e5ef1ee4c709838afcc4a8e27b2?version=3.58.0)


# svelte-confetti-p5 🎊

`svelte-confetti-p5` is an awesome svelte realistic fullpage confetti animation, that can be customized in creative ways like snow flakes, falling balls and more.

---

### p5 what?

No CSS animations, but html Canvas created using [P5.js](https://p5js.org/) (library for creative coding)

### Thanks

- Based on [Tibo](https://gotibo.fr/)'s work on [Codepen](https://codepen.io/elron-the-typescripter/pen/abRNGYy), modified by me to easily control options using Svelte
- We're using [P5-svelte](https://p5-svelte.netlify.app/) to make it work in svelte

<!-- ![Svelte Confetti p5 Example](static/svelte-confetti-p5.gif) -->


## Installation

```bash
# pnpm
pnpm i -D svelte-confetti-p5
```
> pnpm is used here just as an example, you can use your package of choice



## Usage

```html
<script>
  import { ConfettiP5 } from "svelte-confetti-p5";
</script>

<ConfettiP5 />
```

## Customize everything

### Settings
| Prop    |   Type	|   Description |	Default |
|---|---|---|---|
| amount| `number` | Amount of particles | 300
  | colors| `string[]` | Colors of the confetti. Default is very colorful | ["#f44336", "#e91e63", "#9c27b0", "#673ab7", "#3f51b5", "#2196f3", "#03a9f4", "#00bcd4", "#009688", "#4CAF50", "#8BC34A", "#CDDC39", "#FFEB3B", "#FFC107", "#FF9800", "#FF5722"]
  | minSize| `number` | Minimum particle size in pixels | 5
  | maxSize| `number` | Minimum particle size in pixels | 15
  | loop|`boolean` | Rain forever or until all particles fall | false
  | spacing | `number` | Spacing between particles | 10
  | power | `number` | The force of the confetti when it starts | 10
  | shapes | `"squares" \| "circles" \| "mix"` | Particle shapes | "mix"
  | wind|  `number`| Wind from sides | 6
  | weight| `number` | Weight of the particle (this determines the falling speed) | 100
  | rotate| `number` | Rotate particles from side to side because of `wind` | 2
  | flip| `number` | Flip amount to create 3d effect for each particle | 20
  | position|  `"fixed" \| "absolute"` | CSS position of the canvas | "fixed"
  | zIndex| `number` | z-index of the canvas | 999
  | destoryOnFinish | `boolean` | Destroys the canvas when all particles fall (Never gets destroyed when `loop` i set `true`, as expected) | true
  | frameRate | `number` | Refresh rate. Changing this will affect the overall speed of the animation. Recommended to keep as default | 60


  ### Dispatchers
| Dispatcher	|   Description | event returns
|---|---|---|
| `on:start` | Fires when the confetti animation starts | - |
| `on:eachfall` | Fires when a single confetti particle has falled | `on:eachfall=(e => e.detail)` returns the total amount of particles falled so far | |
| `on:done` | Fires when all confetti particles fall | - |
| `on:destroy` | Fires when the canvas is destroyed, automaticlly happens when all particles finished falling | - |


## Examples

### Nice and slow

```html
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
```


### Snow flakes: 
```html
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
```

> ## More examples [here](https://svelte-confetti-p5.netlify.app/)
> ## REPL Playground [here](https://svelte.dev/repl/f68f6e5ef1ee4c709838afcc4a8e27b2?version=3.58.0)



# Used by
[![ConfettiPage.io](static/confettipage-logo.png)](https://confettipage.io)

## License

[MIT license](https://opensource.org/license/mit/)

---
#### Publishing
(Dev note): To publish this library to [npm](https://www.npmjs.com):

```bash
pnpm publish
```


