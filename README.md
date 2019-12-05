# screeps-faq
frequently asked screeps related questions

the purpose of this repo is to gather answers to frequently asked screeps related questions, so that maybe the help channel on the screeps slack will be a little bit less repetitive. the questions should be related to the screeps api, the answers should explain docs, or explain how certain things work server side. sort of like the necessary look behind the scenes, rather than a how to do things ingame. 

hopefully someone will find it useful.

## what is CPU? how does it work?

cpu including the bucket is explained here: https://docs.screeps.com/cpu-limit.html

what isnt really mentioned explicitly but implied on that page is, that 1 CPU is not 1 CPU. since the unit is tied to the execution time on the server, you should only extrapolate CPU improvements on your private server using relative values! (For example if you have a function that takes 1.6 CPU on mmo servers, it might use 1.7 CPU on your private server. you should still be able to roughly estimate your improvement by using relative values (10 % faster on private server should also be roughly 10 % faster on mmo)

the simulator is a "complete" implementation of the private server in your browser, which means it runs very inconsistently so you cannot really use it to check CPU improvements.

## can i save things between ticks? / how do i cache things?

the easiest way to store things between ticks is using Memory. anything you put in Memory gets serialized (via JSON.stringify) at the end of your tick, and deserialized (via JSON.parse) as soon as you use Memory for the first time in a tick. due to the way JSON.parse works, information about types will get lost, and you will only get a primitive object from Memory (for example: saving a RoomPosition object results in a {x: number, y: number, roomName: string} object the next tick).

you pay with your CPU for both serialization and deserialization.

since the introduction of isolated VM you can now also safely use the global object (https://nodejs.org/api/globals.html#globals_global) for caching purposes (referred to as heap too). if you save data in global, you need to be careful about game objects. for example saving a Creep object will result in the Creep object not being updated for the next tick automatically ( = "it becomes stale"), and as such using that same object might result in weird interactions. it is your responsibility to keep things in the heap up to date. 

your global object resets sometimes. this happens randomly every couple thousand of ticks (sometimes you get lucky and even get 50k or more ticks), when you upload code or when you call Game.cpu.halt(). it is often referred to as "global reset".

this is a very cheap method of caching, since you just pay in heap size (if the heap gets big, garbage collection might kick in). so a lot of people cache most of the date here that can be recalculated easily (ranges, paths, ...). 

another use case: you can also use segments as safety option for long term intel or similar data and load all your segments into the heap on global reset, this way freeing up your segment requests and deserialization costs.


## whats the strange _ thing? / what is lodash?

lodash is a library of utility scripts for javascript projects. it is used a lot in the server code, you can recognize it by its shorthand _ (low dash, hence the name). lodash bundles a lot of functionality that can be useful, it also deals with a lot of javascripts weirdness for you (for example when iterating keys of objects). 

all this extra utility comes at a price though: lodash uses a lot of checks to provide its funtionality, some of which you might not need in your exact situation. you will still pay the CPU to run them though. 

a lot of the time you will see players recommend that you implement your own versions of the lodash functionality using the new es6 builtin functions.

the lodash version that comes bundled with screeps is https://lodash.com/docs/3.10.1. you can import newer versions on your own, keep in mind though that you pay for this in code size (although with the new 5 mb limit this isnt as much of an issue anymore).
