
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
    const remainingTime = Math.max(TICK_INTERVAL - elapsedTime, 0);

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
Inside the `handleTickUpdate` is where we will put our preplace; This loop can also be used for many other things. 

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

NOTE: This loop is not EXACTLY 1000/9, but it will never go too fast, in testing results varied and range was between 111 <= x <= 118. This still cuts off:
`window.pingTime - (window.pingTime - (1000/9 - x))`. `x` being interval of each tick in our seperate loop.


#### Preplace Psuedo

```js
/*
 * Assume basic variables are global & you have created a seperate time loop
 */
function handleTickUpdate() {
    const currentTime = performance.now();

    // QUEUED ACITONS:
    handleQueuedActions(); // Handle queued actions for place (that need a sperate loop and don't need player value updates)

    // PREPLACE:
    for (let i = 0, building; i < buildings.length; i++) {
        const { health } = building = buildings[i];

        if (health <= maxPlayersDamage) { //Assume `maxPlayerDamage` is defined and inlcudes all player damage
            //^ Also assume that they hit this tick, replace next tick
            queueNextTick(() => { replace(building) }); //Assume replace & queueNextTick are defined
            //^ this queues into this seperate time loop NOT *NT*
        };
    };
  
}
```

#### How this works

Its really simple, if you can't read code just get off this page. Bascsailly checks for all buildings, checks for their health and if they are breakable next tick, it queues an action to replace the object.

`handleQueuedActions` queues all the actions that were queued for that specific tick using `queueNextTick`. Note that `queueNextTick` is specifically used for this timeloop, do not call your own `queueNextTick`.
