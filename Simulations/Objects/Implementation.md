# Implementation

Implementation is the hard part, having to incoperate a entire velocity system is laborious work; heres a way to do the kb without a vel system.

When creating a temp velocity system, to save the varaible we can either save it as a global, or recurisvly call it. 

This is an example of storing the velocity globaly and then changing it during the collision call.
```js
// Global
let player = {
  velX: 0,
  velY: 0,
};

// SIMULATION:
while(Math.abs(player.velX) > 0 || Math.abs(player.velY) > 0) {
  // DECEL:
  player.velX = player.velY *= 0.993;
}
```

This next example shows a recursive call and returns a final position if velocities are <= 0.

```js
function checkCollision(player, other, velocity) {
  let { velX, velY } = velocity;
  if (Math.abs(velX) > 0 || Math.abs(velY) > 0) {
    // Collision checks:

    //
    return checkCollision(player, other, { velX * 0.993, velY * 0.993 });
  };
  return {...player, x: player.x + (velX * delta), y: player.y + (velY * delta) };
};
```
