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
      
      return checkCollision(player, other, { velX * 0.993, velY * 0.993 });
    } else {
      //you can either loop through buildings here, or recall the entire funciton in newtick.
      //for this example i will loop through buildings here to save space and show functionality. NOTE: this may be recource intensive.
      let buildings = buildings.filter(building => {
        if (collisionDetection({ x: player.x + (velX * delta), y: player.y + (velY* delta) }, player.scale + otherScale) {
          return true
        };
      });
      for (let i = 0, building; i < buildings.length; i++) {
        checkCollision(player, building = buildings[i], { velX * 0.993, velY * 0.993 } );
      };
  };
  return {...player, x: player.x + (velX * delta), y: player.y + (velY * delta) };
};
```

## Understanding the function
1. 



```
