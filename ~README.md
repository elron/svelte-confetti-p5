[![Netlify Status](https://api.netlify.com/api/v1/badges/6fca70dc-1bdc-46a8-b30e-256e69b3c657/deploy-status)](https://app.netlify.com/sites/svelte-animated-headline/deploys)
![npm](https://img.shields.io/npm/dw/svelte-animated-headline)
![npm](https://img.shields.io/npm/v/svelte-animated-headline)

[DEMO](https://svelte-animated-headline.netlify.app/) | [REPL](https://svelte.dev/repl/1ac4e3559ac445ef984d312d632e6f02?version=3.46.4)


# Svelte Confetti p5

[description]

![Svelte Confetti p5 Example](static/svelte-animated-headline.gif)


## Installation

```bash
# pnpm
pnpm i -D svelte-animated-headline
```
> pnpm is used here just as an example, you can use your package of choice



## Usage

### 1. Import:
```html
<script>
  import { AnimatedHeadline } from "svelte-animated-headline";
</script>
```


### 2. Use:

```html
<AnimatedHeadline texts={["Change this", "to whatever", "you like!"]} />
```

## Props

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
  | position|  `"fixed" \| "absolute"` | Css position of the canvas | "fixed"
  | zIndex| `number` | z-index of the canvas | 999
  | destoryOnFinish | `boolean` | If `loop === false`, destroys the canvas when all particles fall | true
  | frameRate | `number` | Refresh rate. Changing this will affect the overall speed of the animation. Recommended to keep as default | 60


  ### Dispatchers
| Dispatcher	|   Description |
|---|---|
| `on:destroy` | Fires when the canvas is destroyed. (Only occurs when all particles fall and `loop === false`) |



> ### Note / Warning
> Each text will be shown as a single-line. No line break support.


## Examples

```html
<ConfettiRain
  colors={["#e84b5e", "#f9db05", "#fefffc", "#ffbe24"]}
  minSize={5}
  maxSize={25}
  spacing={15}
  amount={80}
  flip={2}
  power={5}
  weight={200}
/>
```

```html
<h3>
  I <AnimatedHeadline
    texts={[
      "believe I can fly",
      "can touch the sky",
      "think about it every night and day",
      "spread my wings and fly away",
    ]}
    y={-80}
    fade={2300}
    slide={1000}
    wait={500}
  />... 🎵
</h3>
```

> For more code examples and playground: [REPL](https://svelte.dev/repl/1ac4e3559ac445ef984d312d632e6f02?version=3.46.4)


# Used by
[![ConfettiPage.io](static/confettipage-logo.png)](https://confettipage.io)

## License

[MIT license](https://opensource.org/license/mit/)

#### Publishing
(Dev note): To publish this library to [npm](https://www.npmjs.com):

```bash
pnpm publish
```


