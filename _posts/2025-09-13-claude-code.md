---
layout: post
title:  "Claude Code"
excerpt: "My experience using Claude Code"
img: "/assets/claude-code/thumb.png"
date: 2025-09-13 16:00:00
tags: [code]
---

With all the amazing reviews of [Claude Code](https://www.anthropic.com/claude-code) (CC), I cannot wait to try it myself. I have two use cases.
The first one is to clean up and refactor an old code base of mine, which implements a Penrose tiling game.
The other is to help me learning [LangChain](https://www.langchain.com/) and [LangGraph](https://www.langchain.com/langgraph) by writing a simple conversation agent.

#### Refactor Penrose Tiling
This task is to work in my [penrose-diy](https://github.com/fanyangxyz/penrose-diy) repo.
I gave CC some brief instructions: "refactor and clean up this repo. use the best javescript coding practices". It came up with following issues:

  1. sketch.js:402 - Large monolithic file with multiple responsibilities
  2. sketch.js:14,393 - Mutable global constants (GAMMAS array modified in keyPressed)
  3. sketch.js:31-36 - Global state variables scattered throughout
  4. sketch.js:118-128 - Inconsistent error handling (null checks)
  5. utils.js:69,82 - Functions duplicate p5.js built-in functionality
  6. General - No module structure, everything in global scope
  7. Performance - Multiple nested loops without optimization
  8. Code style - Inconsistent naming and organization

I agreed with all the issues CC identified and really looked forward to how it would address them. 
Below are the to-dos it generated. 
![todos_approach_one](../../../../assets/claude-code/todos_approach_one.png)

After several minutes, it came up with a large commit that added about 800 lines and deleted 250 lines.
You can see all the changes [here](https://github.com/fanyangxyz/penrose-diy/commit/e3ba1de504314afb0ec404a7ec59ffb96973bb43).
Unfortunately, these changes broke the game. It no longer worked. 
There were only grid lines drawn but not rhombuses.

The changes were too long for me to carefully review so I only skimmed them.
CC added some functionalities (e.g. toggle to show the grid) that were not there originally, which I found unnecessary and disobeying the instructions.
Overall the code structure looked clean and organized.
I don't have much experience writing JaveScript code so cannot comment too much on the style.
It was a shame that the refactored code was incorrect.

To better understand CC's ability, I tried an alternative approach.
Instead of using very brief instructions, I decided to guide it through the code base and give specific tasks for each individual files.

I first asked it to fix `utils.js`. It planned the following these steps.
![todos_approach_two](../../../../assets/claude-code/todos_approach_two.png)

Very quickly, it finished the task and summarized the results below.
![results_approach_two](../../../../assets/claude-code/results_approach_two.png)
This time it worked! Nothing was broken and the game worked properly as before.
All the changes can be found [here](https://github.com/fanyangxyz/penrose-diy/commit/e695fe466c8d41d604b4d0d67c7b21714dfad462)
with 243 lines of addition and 148 lines of deletion.
Compared to the approach above, this is more manageable to review.
However, I was a bit disappointed at the changes CC made.
They were too cosmetic in my opinion.
I was hoping CC would have come up with better data structures to store the rhommbus objects.
Because I was new to JaveScript, I'm pretty sure I didn't use the data structures according to the best practices,
so there was definitely room for improvement that was not discoverred by CC.

Then I repeated the same process for `sketch.js`.
Unfortunately the game was again broken by the changes from CC.
So I asked it to reflect and find bugs.
It was able to localize the issues to variable name mismatch and fixed them.
I was a bit surprised at how easy the mistakes were and wondered why it did not catch them the first time.
This is an interesting lesson that sometimes when you attempted to do too much at once,
even the smallest things could go wrong. 

#### Demonstrate LangChain and LangGraph
Details coming soon!
