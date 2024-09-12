# Concept

### What is preplace?
Don't need a bullshit explination: "pre" is a prefix meaning before => beforeplace; placing before an object is broken. Simple.

Preplace might sound contradictory but its not, understanding [delta time](https://en.wikipedia.org/wiki/Delta_timing) and how [servers work](https://en.wikipedia.org/wiki/Game_server) is a pretty good starting foundation to build. To sum it all up: Sync up the client (Refered to as: CL) with the the server (Refered to as: SR) seperate from newTick (refered to as: NT).

#### Preplace vs Replace
Whats the difference?
Replace is reactive, Preplace is proactive. Thats it.

#### Common Misconceptions
Putting your "preplace" in *NT*. This acomplishes nothing as receiving *NT* is depedent on your latency. 
Furhter, using the "renderticks" (requestAnimFrame) to create a preplace. This method can be done, but:
1. Timing Precision: `requestAnimFrame`, is not designed for tasks that need specific timing, and its not a precise/fixed interval
2. Complexity in Sync-tion: Managing and synchornizing  `requestAnimFrame` With *NT* can become cumbersome.

### Concept Overview

#### Core Idea

Preplace is a proactive replacement system designed to address the limitations of traditional reactive replacement methods. Unlike conventional approaches that wait for an object to break before initiating a replacement process, Preplace anticipates it. Preplace aims to minimize downtime and improve efficiency by ensuring that replacements are prepared and executed before the actual object is broken. 

#### Psuedo

To create the seperate loop, try to create a benchmark that is stable everytime, either creating the loop on spawn, on load, on connect etc. For this exmaple I will be doing it on spawn. 

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
}
// ON SPAWN:
function spawnPlayer() {
   tickLoop();
};

```

This creates a seperate loop through the use of `Promise`s and `performance.now()` to handle periodic updates with precise timing control. 

#### How it works

1. Delay Function
   * A custom `delay` function is defined to return a `Promise` that resolves after a specified duration (1000/9), implemented using `setTimeout`.This function is used to create non-blocking puases in the loop, simulating the behavior of the server.
2. Loop Execution
   * The `tickLoop` function that:
        1. Records current tick
        2. Calls the `handleTickUpdate` function
           * This function is where your preplacer or anything else that needs a seperate loop to function.
        3. Elapsed time is calculated
        4. The `delay` function is used to wait for the remaining time, looping the loop at consistent intervals despite variations in processing time.
3. Precise Timing
   * By calculating the remaining time to wait and ajusting for the time taken by `handleTickUpdate`, the loop aims to keep ticks as consistent as possible.

