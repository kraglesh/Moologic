# Concept

#### What is preplace?
Don't need a bullshit explination: "pre" is a prefix meaning before => beforeplace; placing before an object is broken. Simple.

This might look contradictory but its not, understanding [delta time](https://en.wikipedia.org/wiki/Delta_timing) and how [servers work](https://en.wikipedia.org/wiki/Game_server) is a pretty good starting place to look. But to sum it all up: Sync up the Client tick with the Server tick (1000/9) in a seperate time loop (newTick) to remove client latency.

#### Preplace vs Replace
Whats the difference?
Replace is reactive, Preplace is proactive. Thats it.

#### Common Misconceptions
