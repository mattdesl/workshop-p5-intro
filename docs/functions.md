#### <sup>:closed_book: [Intro to Creative Coding](../README.md) → Functions</sup>

---

## Functions

This is a quick intro or "cheat sheet" on writing functions in JavaScript.

## functions are for running blocks of code

In p5.js you have some functions already, like `draw`:

```js
function draw () {
  // ...
}
```

You can write your own functions to split up your code into smaller pieces. This makes it easier to reconfigure your program later.

```js
function draw () {
  // I'm drawing a house
  drawHouseRoof();
  drawHouseDoors();
}

function drawHouseRoof () {
  // maybe its a triangle?
}

function drawHouseDoors () {
  // seems rectangular
}
```

## functions are also for parametric things

Let's say you are drawing a triangle like this:

```js
function draw () {
  const x = width / 2;
  const y = height / 2;
  const size = 40;
  triangle(
    x, y - size / 2,
    x - size / 2, y + size / 2,
    x + size / 2, y + size / 2
  );
}
```

What if you want to draw it again somewhere else? It gets tedious to copy and paste all that code! Instead you can make a function out of it, and include *parameters* in your function so that you can place it differently each time:

```js
function draw () {
  // draw it a few times!
  easyTriangle(width / 2, height / 2, 20);
  easyTriangle(0, 0, 50);
  easyTriangle(width / 4, 0, 60);
}

// when we define the function,
// we say it needs the 'parameters' x, y, size
function easyTriangle (x, y, size) {
  triangle(
    x, y - size / 2,
    x - size / 2, y + size / 2,
    x + size / 2, y + size / 2
  );
}
```

Notice the way we define the `easyTriangle` function—it accepts `x`, `y`, and `size` parameters.

## functions are handy for timers

Want to randomize the background every 2 seconds? You can use `setInterval` and a function for that:

```js
setInterval(randomizeBackground, 2500);

function randomizeBackground () {
  // choose a random color from a list
  const color = random([ 'red', 'blue', 'green' ]);
  // set the page background
  background(color);
}
```

## functions can return values

You can `return` a value to use it elsewhere:

```js
function randomRange (maximum) {
  return Math.random() * maximum;
}

// random between 0..100
console.log(randomRange(100));

// random between 0..0.5
console.log(randomRange(0.5))
```

## functions can be arrows

Another syntax you will see is a shorthand like this:

```js
// declare the function
const myFunc = () => {
  console.log('hi');
};

// call the function
myFunc();
```

This is a different way of writing the following:

```js
// declare the function
function myFunc () {
  console.log('hi');
}

// call the function
myFunc();
```

Other ways that arrow functions might appear:

```js
const hello = () => 'world';
const blah = (a, b) => console.log(a, b);
const myCircle = diameter => circle(0, 0, diameter);
```

## functions can replace loops

We've been writing `for` loops a lot, but you can often use functions to replace them. For example, to iterate over an array:

```js
const array = [ 'red', 'blue', 'green' ];

// This prints each color
array.forEach(color => {
  console.log(color);
});
```

## functions can be parameters

🤯

You can pass a function *as a parameter* to another function.

This is common in `express`, `socket.io` and similar libraries:

```js
server.listen(8000, () => {
  console.log('Server is ready!');
})
```

## more details

For more complete guides, see:

- [Functions Explained](https://www.codeanalogies.com/javascript-functions-explained)
- [Understanding Functions in JavaScript](https://www.taniarascia.com/how-to-define-functions-in-javascript/)

## 

#### <sup>[← Back to README](../README.md)