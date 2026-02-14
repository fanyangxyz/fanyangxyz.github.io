---
layout: post
title:  "Penrose III"
excerpt: ""
img: "/assets/penrose-iii/thumb.png"
date: 2026-02-13 16:00:00
tags: [code, math]
---

There is the third post I have about drawing [Penrose tiling](https://en.wikipedia.org/wiki/Penrose_tiling).
The previous two are [Penrose](/penrose/) and [Penrose II](/penrose-ii/).
There are **three** things that are different in this third version.
First, the algorithm in this version draws the rhombuses directly,
instead of moving them to form a tiling (like [Penrose II](/penrose-ii/)).
Second, the [code](https://github.com/fanyangxyz/penrose-codex) in this version was written by
[Codex](https://openai.com/codex/) with very few instructions.
I mainly just told it to use the [de Bruijn's method](http://www.neverendingbooks.org/de-bruijns-pentagrids/).
And it was able to search the web and used its common sense knowledge to get the correct implementation.
I found this quite impressive given that I had to give [Claude Code](https://code.claude.com/docs/en/overview)
step-by-step instructions in [Penrose II](/penrose-ii/).
Lastly, we can adjust a hyperparameter (the number of families) to get different tilings using the same algorithm.
You can try it yourself [here](https://fanyangxyz.github.io/penrose-codex/). 
Just use the arrow keys to see the effects.
Below are the tilings with the number of families range from three to ten.
When it is three, four, six, and eight, the generated tilings are periodic.

<div class="art penrose-grid">
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-3.png" alt="Penrose tiling with 3 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-4.png" alt="Penrose tiling with 4 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-5.png" alt="Penrose tiling with 5 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-6.png" alt="Penrose tiling with 6 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-7.png" alt="Penrose tiling with 7 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-8.png" alt="Penrose tiling with 8 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-9.png" alt="Penrose tiling with 9 families" />
  </div>
  <div class="penrosepiece">
    <img src="/assets/penrose-iii/penrose-pentagrid-10.png" alt="Penrose tiling with 10 families" />
  </div>
</div>
