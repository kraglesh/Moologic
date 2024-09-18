# Implementation

Implementation is the hard part, having to incoperate a entire velocity system is laborious work; heres a way to do the kb **without** a velocity system.

When creating a temp velocity system, to save the varaible we can either save it as a global, or recursively call it. 

This is an example of storing the velocity globaly and then changing it during the collision call.
```js
// Global
let player = { //Example velocities
  velX: 1,
  velY: 1,
};

// SIMULATION:
while(Math.abs(player.velX) > 0 || Math.abs(player.velY) > 0) {
  // Collision checks:


  //  
  // DECEL:
  player.velX = player.velY *= 0.993;
}
```

This next example shows a recursive call and returns a final position if velocities are <= 0.

```js
function checkCollision(player, other, velocity = { velX: 1, velY: 1} ) { //Example velocities
  let { velX, velY } = velocity;
  if (Math.abs(velX) > 0 || Math.abs(velY) > 0) {
    // Collision checks:

    //
    return checkCollision(player, other, { velX * 0.993, velY * 0.993 });
  };
  return {...player, x: player.x + (velX * delta), y: player.y + (velY * delta) };
};
```


To incoperate this system, we will have to implement how the player moves based on colliding with an object. For these examples I will use the recursively called function instead of the global one, but the logic is the same for both.

```js
/*
 * Assume basic variable usage and def
*/

function checkCollision(player, other, velocity = { velX: 1, velY: 1} ) { //Example velocities
  let { velX, velY } = velocity;
  player.predictedX = player.predictedY = null; //make fasly to use ??
  //doesn't matter if we remove predictedx/y because player will be sent with it previous recursive call
  let otherScale = (other.getScale ? other.getScale() : other.scale);
  if (Math.abs(velX) > 0 || Math.abs(velY) > 0) {
    if (collisionDetection(player, other, player.scale + otherScale)) {
      let tmpDir = UTILS.getDirection(player.x, player.y, other.x, other.y);
      player.predictedX = other.x + (otherScale * cos(tmpDir));
      player.predictedY = other.y + (otherScale * sin(tmpDir));
      velX = velY * 0.75; //velocity loss on collision (check bundle)

      if (other.ignoreCollision) { //trap
        return player
      };

      if (other.dmg && !isTeam(other.owner, player)) { //if the object that the player is colliding into is a spike and not a team memebrs spike
        velX += 1.5 * cos(tmpDir); //check bundle
        velY += 1.5 * sin(tmpDir);
      };
      
      return checkCollision( {...player, x: player.predictedX ?? player.x, y: player.predictedY ?? player.y }, other, { velX * 0.993, velY * 0.993 });
    } else {
      //you can either loop through buildings here, or recall the entire funciton in newtick.
      //for this example i will loop through buildings here to save space and show functionality. NOTE: this may be recource intensive.
      let buildings = gameObjects.filter(building => collisionDetection({ x: player.x + (velX * delta), y: player.y + (velY * delta) }, building, player.scale + (building.getScale ? building.getScale() : building.scale));
      for (let i = 0, building; i < buildings.length; i++) {
        checkCollision({...player, x: player.x + (velX * delta), y: player.y + (velY * delta) }, building = buildings[i], { velX * 0.993, velY * 0.993 } );
      };
  };
  return {...player, x: player.predictedX ?? player.x + (velX * delta), y: player.predictedY ?? player.y + (velY * delta) };
};
```


## Understanding the function
1. First it checks the velocity of inputed parameter.
2. Checks if the player will collide with an object without velocity.
3. Then finds the new position the player will end up if it collides with something, and reduces velocity.
4. Checks if `other` is a trap, stops the entire recusive loop.
5. Checks if `other` is a spike, adds velocity based on direction `player` hits the trap
6. recursively calls the function to decel velocity, and see if another object will collide with `player`.
7. Finally, it returns the final player position after all recursive calls.



# Finding Velocity Using Final Position

To calculate the velocity of an object using its final position, you can use the following formula:

## Formula

1. **Determine the Change in Position:**
   - Calculate the change in position in the x-direction (∆*x*) and the y-direction (∆*y*):
     - ∆*x* = *x*<sub>*final*</sub> - *x*<sub>*initial*</sub>
     - ∆*y* = *y*<sub>*final*</sub> - *y*<sub>*initial*</sub>
     
NOTE: *coord*<sub>*final*</sub> can be the current position and *coord*<sub>*initial*</sub> can be the last tick position (lastX/Y).


2. **Determine the Time Interval:**
   - Identify the time interval (*t*) over which the motion occurs.
     - In this scenario, the `delta` or *t* is $\frac{500}{9}$ (timeBetweenTick / 2).

3. **Calculate Velocity Components:**
   - **Velocity in the x-direction (vx):**
     - *v*<sub>*x*</sub> = $\frac{∆x}{t}$

   - **Velocity in the y-direction (vy):**
     - *v*<sub>*y*</sub> = $\frac{∆y}{t}$


