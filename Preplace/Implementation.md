
#### Seperate Loop Psuedo

To create the seperate loop, try to create a foundation that is stable everytime, either creating the loop on spawn, on load, on connect etc. For this exmaple I will be doing it on spawn. 

```js
const TICK_INTERVAL = 1000 / 9;

function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function handleTickUpdate() {
  const currentTime = performance.now();

  // PREPLACE:
}

function tickLoop() {
  const startTime = performance.now();

  handleTickUpdate();

  const elapsedTime = performance.now() - startTime;
  const remainingTime = Math.max(TICK_INTERVAL_MS - elapsedTime, 0);

  delay(remainingTime).then(() => {
    tickLoop(); // Call the loop again after the delay
  });
};

// ON SPAWN:
function spawnPlayer() {
   tickLoop();
};

```

This creates a seperate loop through the use of `Promise`s and `performance.now()` to handle periodic updates with precise timing control. 

#### How it works

1. Delay Function
   * A custom `delay` function is defined to return a `Promise` that resolves after a specified duration (1000/9), implemented using `setTimeout`.This function is used to create non-blocking puases in the loop, simulating the behavior of the server.
2. Loop
   * The `tickLoop` function that:
        1. Records current tick
        2. Calls the `handleTickUpdate` function
           * This function is where your preplacer or anything else that needs a seperate loop to function.
        3. Elapsed time is calculated
        4. The `delay` function is used to wait for the remaining time, looping the loop at consistent intervals despite variations in processing time.
3. Precise Timing
   * By calculating the remaining time to wait and ajusting for the time taken by `handleTickUpdate`, the loop aims to keep ticks as consistent as possible.
  
#### Preplace Psuedo

```js
/*
 * Assume basic variables
 */
function handleTickUpdate() {
  const currentTime = performance.now();

  // PREPLACE:
  
}
```

Inside the handleTickUpdate is where we will put our preplace, this loop cna also be used for many other things. 
