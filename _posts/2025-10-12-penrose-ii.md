---
layout: post
title:  "Penrose II"
excerpt: "Automatic Penrose tiling using the pentagrid method."
img: "/assets/penrose-ii/thumb.png"
date:   2025-10-12 05:15:00
tags: [code, math]
---

tl;dr I finally figured out how to algorithmically align the rhombuses to form a Penrose tiling!
The main idea is the classic [breadth-first search](https://en.wikipedia.org/wiki/Breadth-first_search).

<div class="art">
  <div class="penrosepiece">
    <img src="/assets/penrose-ii/penrose-alignment.gif" alt="Penrose animation" />
    <div class="caption">Automatic rhombus alignment using breadth-first search</div>
  </div>
</div>

The source code is [here](https://github.com/fanyangxyz/penrose-diy).
The animation you see here can be found on the `visualization` branch.


#### Background

This is a follow-up on my previous post [Penrose](/penrose/), 
where I used de Bruijn's pentagrid method to generate Penrose tilings.
However, I only successfully placed the rhombuses at the intersection of the grid lines.
But these were only the initial locations of the rhombuses. 
We still need to align them.
I didn't figure out a way to automatically do this.
So I ended up making it a "game", where people can use the mouse to drag rhombuses and manually align them.
You can try it [here](https://fanyangxyz.github.io/penrose-diy/).
However, after ten months of pondering, now there is a "Align Rhombuses" button, 
which can do the job for you!

<div class="art">
  <div class="crochetpiece">
    <img src="/assets/penrose-ii/before.png" alt="Penrose game before" />
    <div class="caption">Before clicking "Align Rhombuses"</div>
  </div>
  <div class="crochetpiece">
    <img src="/assets/penrose-ii/after.png" alt="Penrose game after" />
    <div class="caption">After clicking "Align Rhombuses"</div>
  </div>
</div>

A special thank you to [Professor Kaplan](https://cs.uwaterloo.ca/~csk/)
who replied to my emails with the idea of using breadth-first search and debugging suggestions.
The moment I connected all the dots was when reading this [math StackExchange post](https://math.stackexchange.com/questions/3465244/properly-drawing-a-penrose-tiling-using-the-pentagrid-method).
Knowing that someone has done this successfully before greatly boosted morale.


#### Algorithm
Because we are going to use the breadth-first algorithm, the first step is to construct a graph.
The vertices of the graph are rhombuses.
There is an edge between the two vertices if and only if the rhombuses are "next to each other".
Specifically, two rhombuses are next to each other if they are on the same grid line 
and there are no other rhombuses between them on that grid line.
Now with this graph, we start with any initial rhombus, find all its four (there are always four) neighbors.
We move the neighbors so that their "closest" edges are aligned.
Closest here cannot always be measured by the Euclidean distance because the rhombuses might overlap.
The closest edges are defined once we create an edge between two rhombuses `R_x` and `R_y`.
Without loss of generality, we assume that `R_x` is to the "left" of `R_y`,
therefore, the "right" edge of `R_x` needs to be aligned with the "left" edge of `R_y`.
I couldn't find a more rigorous mathematical way to describe this.
The idea is to use the grid lines of the intersection that each rhombus lies on to 
determine the relative position of them.
I hope the intuition is clear here.
After this step, we iterate over newly aligned rhombuses in a first-in-first out order, 
finding and moving their neighbors.


#### Implementation
I used Claude Code (CC) a lot. 
I even hit the daily usage quota because I only have a normal subscription account.
However, I had to give __VERY__ specific instructions.
Giving the high level idea, such as "next to each other", did not work.
CC chose to use the distance between vertices, 
which is not always correct because sometimes the rhombuses overlap.
Similarly, I had to explicitly state the definition of the "closest edges".
Unlike other tasks I've worked on with CC where it was able to figure out the solution given very high-level instructions,
I needed to "micro-manage" it this time.
This is probably because Penrose tiling is not a very frequent topic in its training data.
And it hasn't seen the exact code I wanted and couldn't retrieve the solutions from memory.
This is the [commit](https://github.com/fanyangxyz/penrose-diy/commit/6f1f3743de48b54709fbf3caac2db8531ee9ed95) where
CC finally implemented everything exactly as I wanted.
To be honest I didn't review the code too carefully since the results looked correct.
One day I will come back and read/study the code CC wrote to improve my JavaScript coding ability.
