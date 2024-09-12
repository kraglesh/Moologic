# Concept

### What is preplace?
Don't need a bullshit explination: "pre" is a prefix meaning before => beforeplace; placing before an object is broken. Simple.

This might look contradictory but its not, understanding [delta time](https://en.wikipedia.org/wiki/Delta_timing) and how [servers work](https://en.wikipedia.org/wiki/Game_server) is a pretty good starting foundation to build. To sum it all up: Sync up the client (Refered to as: CL) with the the server (Refered to as: SR) seperate from newTick (refered to as: NT).

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
