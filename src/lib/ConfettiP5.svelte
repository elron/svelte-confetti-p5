<script lang="ts">
  import P5 from "p5-svelte";
  import { createEventDispatcher, onDestroy, onMount } from "svelte";

  export let w = 0;
  export let h = 0;
  export let amount = 300;
  export let loop = false;
  export let minSize = 5;
  export let maxSize = 15;
  export let wind = 6;
  export let power = 10;
  export let spacing = 10;
  export let frameRate = 60;
  export let shapes: "squares" | "circles" | "images" | "mix" = "mix";
  export let position: "fixed" | "absolute" = "fixed";
  export let zIndex = 999;
  export let weight = 100;
  export let rotate = 2;
  export let flip = 20;
  export let destoryOnFinish = true;
  export let colors = [
    "#f44336",
    "#e91e63",
    "#9c27b0",
    "#673ab7",
    "#3f51b5",
    "#2196f3",
    "#03a9f4",
    "#00bcd4",
    "#009688",
    "#4CAF50",
    "#8BC34A",
    "#CDDC39",
    "#FFEB3B",
    "#FFC107",
    "#FF9800",
    "#FF5722",
  ];
  export let images: string[]; // array of image URLs
  let imagesLoaded = [];

  const dispatch = createEventDispatcher();

  let p5ref = undefined;

  const sketch = (p5) => {
    let newVector, oldVector, pressure;

    class Particle {
      parent: any;
      gravity: any;
      shape: any;
      step: number;
      take: number;
      takeFactor: any;
      multFactor: any;
      takeAngle: number;
      takeSpeed: number;
      isImage: boolean;
      image: any;
      position: any;
      velocity: any;
      friction: any;
      size: any;
      half: number;
      color: any;
      constructor(parent) {
        this.parent = parent;
        this.gravity = parent.gravity;
        this.reset();
        this.shape =
          shapes === "mix"
            ? p5.round(p5.random(0, 1))
            : shapes === "circles"
            ? 1
            : 0;
        this.step = 0;
        this.take = 0;
        this.takeFactor = p5.random(-0.02, 0.02);
        this.multFactor = p5.random(0.01, 0.08);
        this.takeAngle = 0;
        this.takeSpeed = 0.05;

        this.isImage = false;
        if (imagesLoaded.length) {
          if (shapes === "mix" && p5.random() > 0.5) {
            this.isImage = true;
            this.image = p5.random(imagesLoaded);
          } else if (shapes === "images") {
            this.isImage = true;
            this.image = p5.random(imagesLoaded);
          }
        }
      }
      reset() {
        this.position = this.parent.position.copy();
        this.position.y = p5.random(-20, -100);
        this.position.x = p5.random(0, p5.width);
        this.velocity = p5.createVector(
          p5.random(-wind, wind),
          p5.random(-spacing, power)
        );
        this.friction = p5.random(0.995, 0.98);
        this.size = p5.round(p5.random(minSize, maxSize));
        this.half = this.size / 2;
        this.color = p5.color(p5.random(colors));
      }
      destroy() {
        this.parent.Particles = this.parent.Particles.filter((p) => p !== this);
      }
      draw() {
        this.step =
          flip === 0 ? 1 : 0.5 + Math.sin(this.velocity.y * flip) * 0.5;

        this.take =
          this.takeFactor + Math.cos(this.takeAngle) * this.multFactor;
        this.takeAngle += this.takeSpeed;
        p5.translate(this.position.x, this.position.y);
        p5.rotate(this.velocity.x * rotate);
        p5.scale(1, this.step);
        p5.noStroke();
        p5.fill(this.color);

        if (this.isImage) {
          console.log("Rendering image...");
          // Custom square size
          // p5.image(this.image, -this.half, -this.half, this.size, this.size);

          const imgWidth = this.image.width;
          const imgHeight = this.image.height;
          // Original Image size
          p5.image(
            this.image,
            -imgWidth / 2,
            -imgHeight / 2,
            imgWidth,
            imgHeight
          );
        } else if (this.shape === 0) {
          p5.rect(-this.half, -this.half, this.size, this.size);
        } else {
          p5.ellipse(0, 0, this.size, this.size);
        }

        p5.resetMatrix();
      }
      integrate() {
        // Apply gravity to the velocity
        this.velocity.add(this.gravity);

        // Adjust the x velocity by the 'take' value (this seems to introduce some random motion)
        this.velocity.x += this.take;

        // Apply friction to the velocity
        this.velocity.mult(this.friction);

        // Update the position using the velocity
        this.position.add(this.velocity);

        // Check if the particle is an image
        if (this.isImage) {
          // For image particles, we need to account for their size when checking for out-of-bounds conditions

          // Check if the top of the image has gone past the bottom of the screen
          if (this.position.y - this.image.height / 2 > p5.height) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }

          // Check if the right edge of the image has gone past the left of the screen
          if (this.position.x + this.image.width / 2 < 0) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }

          // Check if the left edge of the image has gone past the right of the screen
          if (this.position.x - this.image.width / 2 > p5.width) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }
        } else {
          // For non-image particles, we only need to check the central position

          // Check if the particle has gone past the bottom of the screen
          if (this.position.y > p5.height) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }

          // Check if the particle has gone past the left of the screen
          if (this.position.x < 0) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }

          // Check if the particle has gone past the right of the screen
          if (this.position.x > p5.width) {
            fallenParticlesAmount += 1;
            if (loop) this.reset();
            else this.destroy();
          }
        }
      }

      render() {
        this.integrate();
        this.draw();
      }
    }
    class ParticleSystem {
      position: any;
      maxNumber: any;
      gravity: any;
      friction: number;
      Particles: never[];
      constructor(maxNumber, position, gravity) {
        this.position = position.copy();
        this.maxNumber = maxNumber;
        this.gravity = p5.createVector(0, weight / 1000);
        this.friction = 0.98;
        // le tableau
        this.Particles = [];
        for (var i = 0; i < this.maxNumber; i++) {
          this.Particles.push(new Particle(this));
        }
      }
      render() {
        if (pressure) {
          var force = p5.Vector.sub(newVector, oldVector);
          this.gravity.x = force.x / 20;
          this.gravity.y = force.y / 20;
        }

        this.Particles.forEach((Particles) => Particles.render());
      }
    }
    let confetties;

    p5.preload = () => {
      if (images?.length) {
        for (let i = 0; i < images.length; i++) {
          imagesLoaded[i] = p5.loadImage(images[i]);
        }
      }
    };

    p5.setup = () => {
      p5.createCanvas(w || p5.windowWidth, h || p5.windowHeight);
      p5.frameRate(frameRate);
      oldVector = p5.createVector(0, 0);
      newVector = p5.createVector(0, 0);
      confetties = new ParticleSystem(
        amount,
        p5.createVector(p5.width / 2, -20)
      );

      p5ref = p5;
    };

    p5.draw = () => {
      p5.clear();
      //   p5.background(p5.color("#111"));
      newVector.x = p5.mouseX;
      newVector.y = p5.mouseY;
      confetties.render();
      oldVector.x = newVector.x;
      oldVector.y = newVector.y;
    };

    p5.windowResized = () => {
      p5.resizeCanvas(w || p5.windowWidth, h || p5.windowHeight);
      confetties.position = p5.createVector(p5.width / 2, -40);
    };

    // p5.mousePressed = () => {
    //   p5.next = 0;
    //   pressure = true;
    // };

    // p5.mouseReleased = () => {
    //   pressure = false;
    //   confetties.gravity.y = 0.1;
    //   confetties.gravity.x = 0;
    // };
  };

  let fallenParticlesAmount = 0;

  $: hasFinished = fallenParticlesAmount >= amount;

  $: shouldShow = !hasFinished || !destoryOnFinish || loop;

  onMount(() => {
    dispatch("start");
  });

  onDestroy(() => {
    if (!shouldShow) dispatch("destory");
  });

  // fixes memory leak
  // https://github.com/tonyketcham/p5-svelte/issues/322
  // https://github.com/mlhoutel/garden/blob/1024a14e66b9cc37536795fac18b08c8e46dc06f/src/library/components/projects/Canvas.svelte#L31-L36
  onDestroy(() => {
    if (p5ref) {
      p5ref.remove();
      p5ref = undefined;
    }
  });

  $: {
    dispatch("eachfall", fallenParticlesAmount);
  }

  $: {
    if (hasFinished) {
      dispatch("done");
    }
  }
</script>

<!-- {fallenParticlesAmount}/{amount}
{hasFinished} -->

{#if shouldShow}
  <!-- <h1>CANVAS</h1> -->
  <div
    class="confetti-p5-container"
    style="pointer-events:none; position: {position};z-index: {zIndex};"
  >
    <P5 {sketch} />
  </div>
{/if}

<style>
  div {
    top: 0;
    left: 0;
  }
</style>
