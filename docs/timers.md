#### <sup>:closed_book: [Intro to Creative Coding](../README.md) → Timers</sup>

---

# Timers

## run something every N seconds

We set an `setInterval` to run a function every N seconds.

```js
// 2.0 seconds in milliseconds
const delay = 1500;

// Run the function every N milliseconds
setInterval(changeBackground, delay);

function changeBackground () {
  background('blue');
}
```

## run something after N seconds

We set a `setTimeout` to run a function after a delay.

```js
// 1.5 seconds in milliseconds
const delay = 1500;

// Delay function
setTimeout(changeBackground, delay);

function changeBackground () {
  background('blue');
}
```

## stop an interval

If you set an interval, you can stop it with `clearInterval` like so:

```js
let interval = setInterval(changeBackground, delay);

...

  // Stops the interval
  clearInterval(interval);
```

## stop a timeout

If you set a timeout, you can stop it with `clearTimeout` like so:

```js
let timeout = setTimeout(changeBackground, delay);

...

  // Stops the timeout
  clearTimeout(timeout);
```

## 

#### <sup>[← Back to README](../README.md)