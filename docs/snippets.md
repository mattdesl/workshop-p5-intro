#### <sup>:closed_book: [Intro to Creative Coding](../README.md) → Snippets</sup>

---

# Snippets

Here you will find some 'recipes' and patterns that we'll be using during the workshop.

## Contents

- [Sketch Fundamentals](#sketch-fundamentals)

- [Drawing Polygons](#drawing-polygons)

- [Line Segment With Angle](#line-segment-with-angle)

- [Linear Spacing in Loops](#linear-spacing-in-loops)

- [Trigonometry: Computing a Circle](#trig)

- [Mapping `-1..1` to `0..1`](#mapping0)

- [Mapping `0..1` to `-1..1`](#mapping1)

- [Linear Interpolation within a Range](#lerp)

- [Inverse Linear Interpolation](#inverseLerp)

- [Springing Motion](#springing-motion)

- [Seamless Loops](#seamless-loops)

- [Rotate Around Centre](#rotate-around-centre)

- [Fixed Sized Canvas](#fixed-size-canvas)

- [Fixed Seed](#fixed-seed)

## Sketch Fundamentals

At the core of each p5.js sketch is a setup, resize handler, and render loop like so:

```js
// Create a new canvas to the browser size
function setup() {
  createCanvas(windowWidth, windowHeight);
}

// On window resize, update the canvas size
function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}

// Render loop that draws shapes
function draw() {
  // your drawing code ...
}
```

## Drawing Polygons

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example?path=sketch.js)

Here's a function that you can use to draw a basic polygon in p5.js. This allows you to render triangles, diamonds, and other shapes.

```js
function polygon(x, y, radius, sides = 3, angle = 0) {
  beginShape();
  for (let i = 0; i < sides; i++) {
    const a = angle + TWO_PI * (i / sides);
    let sx = x + cos(a) * radius;
    let sy = y + sin(a) * radius;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}
```

Usage:

```js
// Make radius a fraction of screen size
const radius = Math.min(width, height) * 0.25;

// Do one full rotation each second
const angle = (millis() / 1000) * PI * 2;

// Draw shape
polygon(width / 2, height / 2, radius, 3, angle);
```

## Line Segment With Angle

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-line?path=sketch.js)

Sometimes you just want to draw a little 'tick' or line at a specific `(x, y)` coordinate, with a defined length. Here's a function you can use:

```js
// Draw a line segment centred at the given point
function segment(x, y, length, angle = 0) {
  const r = length / 2;
  const u = Math.cos(angle);
  const v = Math.sin(angle);
  line(x - u * r, y - v * r, x + u * r, y + v * r);
}
```

Usage:

```js
// One full rotation each second
const angle = (millis() / 1000) * PI * 2;

// Length of line segment is a fraction of the screen
const length = Math.min(width, height) / 4;

// Draw segment
segment(width / 2, height / 2, length, angle);
```

## Linear Spacing in Loops

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-repeat?path=sketch.js)

Often you'll need a value that goes from 0 to 1 (inclusive) within a loop. You can use the following pattern which also guards against a count of 1 by using `0.5` (i.e. "center").

```js
// number of objects
const count = 10;

// draw each object
for (let i = 0; i < count; i++) {
  // value will be 0..1 (inclusive)
  const t = count <= 1 ? 0.5 : i / (count - 1);

  // Do something with t...
}
```

This is also the basis for demos like:

- [noise](https://glitch.com/edit/#!/p5-example-noise-lines?path=sketch.js)

- [rings](https://glitch.com/edit/#!/p5-example-rings?path=sketch.js)

- and many others...

<a name="trig"></a>

## Trigonometry: Computing a Circle

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-trig?path=sketch.js)

The equation for a circle looks like this:

```
P = (x, y) + (cos(θ), sin(θ)) * radius
```

Here's how that looks in code:

```js
// Your inputs
const angle = /* some angle in radians */;
const radius = /* some radius */;
const x = /* x position to draw */;
const y = /* y position to draw */;

// Compute a 'unit' circle (length 1)
const u = cos(angle);
const v = sin(angle);

// To get your final position,
// scale and translate the unit circle
const px = x + u * radius;
const py = y + v * radius;
```

<a name="mapping0"></a>

## Mapping `-1..1` to `0..1`

Say you have a number _t_ between -1 and 1 (inclusive) and you want to map it to 0 to 1 (inclusive), you can use this:

```js
const n = t * 0.5 + 0.5;
```

A good example is `sin()` that produces numbers between -1 and 1.

```js
// Current time in seconds
const time = millis() / 1000;

// Value returned is between -1..1
let n = sin(time);

// Map it to 0..1
n = n * 0.5 + 0.5;
```

<a name="mapping1"></a>

## Mapping `0..1` to `-1..1`

Say you have a number _t_ between 0 and 1 (inclusive) and you want to map it to -1 to 1 (inclusive), you can use this:

```js
const n = t * 2 - 1;
```

<a name="lerp"></a>

## Linear Interpolation within a Range

Say you have _t_ between 0 and 1, and you want to map this to a range from 25 to 75. You can use `lerp()` (linear interpolation) to achieve this.

```js
const t = /* value from 0..1 */;

const minVal = 25;
const maxVal = 75;
const x = lerp(minVal, maxVal, t);
```

<a name="inverseLerp"></a>

## Inverse Linear Interpolation

Take the opposite of the above example: say you have _v_ which is some number between your range of 25 and 75, and you want to compute from it a _t_ value betwen 0..1.

```js
function inverseLerp(min, max, current) {
  if (Math.abs(min - max) < 1e-10) return 0;
  else return (current - min) / (max - min);
}
```

Usage:

```js
// Your range & current value
const minVal = 25;
const maxVal = 75;
const current = 30;

// Returns a value from 0..1
const t = inverseLerp(minVal, maxVal, current);
```

## Springing Motion

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-mouse?path=sketch.js)

You can use linear interpolation to spring a value toward another, for example smooth movement toward the mouse. Here's a `spring` utility:

```js
// Springs A toward B with a power, accepting deltaTime
function spring(a, b, power, dt) {
  return lerp(a, b, 1 - Math.exp(-power * dt));
}
```

Example:

```js
// The old (current/previous) value
const a = currentX;

// The target value, e.g. new mouse position
const b = mouseX;

// How fast to spring toward it
const power = 5;

// Delta time in seconds
const dt = deltaTime / 1000;

// Update currentX toward new value
currentX = spring(a, b, power, dt);
```

## Seamless Loops

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-loop?path=sketch.js)

To make seamless loops, you can use the current time in seconds to create a `playhead` variable from 0..1.

```js
// Get time in seconds
const time = millis() / 1000;

// How long we want the loop to be
const duration = 7;

// Get a 'playhead' from 0..1
// We use modulo to keep it within 0..1
const playhead = (time / duration) % 1;
```

Now you can use the `playhead` variable to influence the animations.

```js
// You can use 2PI for seamless rotations
const rotation = playhead * PI * 2;

// Or linear interpolation to animate from A to B
const scale = lerp(0.25, 1.25, playhead);
```

## Rotate Around Centre

> #### ✨ [Live Demo](https://glitch.com/edit/#!/p5-example-rotate-around-center?path=sketch.js)

Imagine you are trying to draw a shape like a rectangle, but you want it rotated around its centre.

```js
const rotation = /* some rotation in radians */;

// Save state
push();

// Translate to centre point and rotate,
// then translate back to previous position
translate(x, y);
rotate(rotation);
translate(-x, -y);

// Draw your shape centred at (x, y)
rectMode(CENTER);
rect(x, y, size, size);

// Restore state
pop();
```

It could also be written as:

```js
...
translate(x, y);
rotate(rotation);

rectMode(CENTER);
rect(0, 0, size, size);
...
```

## Fixed Size Canvas

Let's say you want to export your sketch at exactly 1024x1024 px, you can do this to set the size properly but then also visualize it full-screen in the browser window:

```js
const WIDTH = 1024;
const HEIGHT = 1024;

function fixedCanvas(width, height) {
  resizeCanvas(width, height);
  const style = canvas.style;
  const amt = "90%";
  if (innerWidth / innerHeight > width / height) {
    style.width = "auto";
    style.height = amt;
  } else {
    style.height = "auto";
    style.width = amt;
  }
  style.top = style.left = style.bottom = style.right = "0";
  style.position = "absolute";
  style.margin = "auto";
}

// Create a new canvas to the browser size
function setup() {
  // Create a canvas
  createCanvas();
  // Resize it to your fixed size
  fixedCanvas(WIDTH, HEIGHT);
}

function windowResized() {
  // Resize it to your fixed size
  fixedCanvas(WIDTH, HEIGHT);
}
```

In Chrome you can **Right Click > Save As** on the canvas to get a PNG.

## Fixed Seed

If you want a fixed random seed so that you can reload the page and get a new result, but then re-render a static result without it changing, you can use the following:

```js
let seed;

function setup() {
  seed = floor(random(0, 1e8));
  // Setup your canvas as usual
  // ...
}

function draw() {
  // Static drawing
  noLoop();
  // Reset random to seed
  randomSeed(seed);
}
```

##

#### <sup>[← Back to Documentation](../README.md)
