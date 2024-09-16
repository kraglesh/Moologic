# Concept

The general concept behind a object simulation is where will a player end (End pos: referred to as *EP*) after colliding with object(s).

Sounds simple? because it is simple.

## Benefits

Very helpful for autoplace, spike collisions, your entire KB simulation, and general usage for autopush.


## Prerequisites

A velocity system, and NOT last - current. If your velocity system is last - current and it updates everytick, the velocity during collisions won't do anything because its being reset by server-sent data.
