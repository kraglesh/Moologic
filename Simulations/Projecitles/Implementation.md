# Implementation of projectile simulating

To simulate a projectile, we have to take the speed and how fast it moves per tick.
Then after it moves check if it collides with anything or it reaches its max range.

On projectile creation, a ton of data is send from the server to the client. All varaibles that aren't defined are sent by server and should be saved.

```js
this.simulate = function() { //You can either call this as a seperate function or a function in the projectiles class
  if (!this.active) return; //returns if the projectile is inactive
  let ticks = 0, speed = this.speed * delta, hitObj;
  let velocity = {
    x: cos(this.dir),
    y: sin(this.dir),
  };
  let x = this.x, y = this.y, skipMov = this.skipMov, range = this.range;

  while(this.active) { //runs only if active (doesn't matter whats here just want a while)
    tick++;
    if (skipMov) {
      skipMov = false;
      continue;
    };

    x += speed * velocity.x;
    y += speed * velocity.y;
    if ((range -= speed) <= 0) {
      x += range * velocity.x;
      y += range * velocity.y;
      speed = 1;
      range = 0;
    };

    if (hitObj = (projCollision(...arguments)) || !range) { //make your own projCollision
      return { hitObj, tick }
    };
};
```



This code should be self explanitory. 

NOTE: In my mod, this goes where projectiles are created clientside.

