# screeps-faq
frequently asked screeps related questions

the purpose of this repo is to gather answers to frequently asked screeps related questions, so that maybe the help channel on the screeps slack will be a little bit less repetitive. the questions should be related to the screeps api, the answers should explain docs, or explain how certain things work server side. sort of like the necessary look behind the scenes, rather than a how to do things ingame. 

hopefully someone will find it useful.

## what is CPU? how does it work?

cpu including the bucket is explained here: https://docs.screeps.com/cpu-limit.html

what isnt really mentioned explicitly but implied on that page is, that 1 CPU is not 1 CPU. since the unit is tied to the execution time on the server, you should only extrapolate CPU improvements on your private server using relative values! (For example if you have a function that takes 1.6 CPU on mmo servers, it might use 1.7 CPU on your private server. you should still be able to roughly estimate your improvement by using relative values (10 % faster on private server should also be roughly 10 % faster on mmo)

the simulator is a "complete" implementation of the private server in your browser, which means it runs very inconsistently so you cannot really use it to check CPU improvements.


## whats the strange _ thing? / what is lodash?

lodash is a library of utility scripts for javascript projects. it is used a lot in the server code, you can recognize it by its shorthand _ (low dash, hence the name). lodash bundles a lot of functionality that can be useful, it also deals with a lot of javascripts weirdness for you (for example when iterating keys of objects). 

all this extra utility comes at a price though: lodash uses a lot of checks to provide its funtionality, some of which you might not need in your exact situation. you will still pay the CPU to run them though. 

a lot of the time you will see players recommend that you implement your own versions of the lodash functionality using the new es6 builtin functions.

the lodash version that comes bundled with screeps is https://lodash.com/docs/3.10.1. you can import newer versions on your own, keep in mind though that you pay for this in code size (although with the new 5 mb limit this isnt as much of an issue anymore).
