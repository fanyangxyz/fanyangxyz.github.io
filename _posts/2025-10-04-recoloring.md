---
layout: post
title:  "Recoloring"
excerpt: ""
img: "/assets/recoloring/thumb.png"
date: 2025-10-04 21:00:00
tags: [art,code,ml,math]
---

### Introduction
I saw [this project](https://zhuanlan.zhihu.com/p/695729586) a couple of years ago and found it super interesting.
What it does is swapping the colors of two images in an __aesthetic__ way.
The effect is a color transfer from one image (i.e. source) to another (i.e. target).
Using Pokemons as an example, you can see my reproduced results below:
the orange Charizard is recolored using the major colors in the green Bulbasaur.
We consider this a "good" transfer because the Charizard's body consists of multiple shades of green
and the inside area of its wings still has a different color.
In other words, the color distributions in source and target are "similar". 
(We will formalize this concept later.)
The goal of this project is to **automatically** find such "good" transfers.

<div class='art'>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/charizard_to_bulbasaur_comparison.png" alt="charizard_to_bulbasaur" />
    <div class="caption">Recolor Charizard (left) using the major colors in Bulbasaur (middle)</div>
  </div>
</div>


### Related work
In this era of Generative AI, one might first and naturally ask: can models do this task?
To answer this question, I used ChatGPT by prompting it to generate an Eevee image using the color palette in Jigglypuff.
Below is the result from ChatGPT. 
I think it did a good job in generating a nice-looking pink Eevee.
But it failed to use the blue color in the eyes of Jigglypuff.
<div class='art'>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/chatgpt_comparison.png" alt="ChatGPT result" />
    <div class="caption">Result from ChatGPT</div>
  </div>
</div>

Our algorithms made an interesting and different choice.
It created a blue Eevee with pink fur near its neck and tail tip.
So it used both colors.
But one might argue that the pairing of blue and pink is quite different from the pairing of yellow and brown in the original image.

<div class='art'>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/eevee_to_jigglypuff_comparison_six_colors.png" alt="Eevee to Jigglypuff" />
    <div class="caption">Result from our algorithms</div>
  </div>
</div>

It is subjective which one of the above is better.
Different people have different criteria.
In our algorithms, we formulate what we think is "good" using mathematical concepts.
This is drastically different from the ChatGPT's data-driven approach.

### Algorithms
There are two steps.
First is to extract the major colors from an image.
Second is to find a "good" permutation mapping between two color palettes.

#### Palette extraction
The high-level idea is to decompose the image into a weighted linear combination of color layers.
I initially tried K-means (a clustering algorithm) for this task.
The color of each pixel is then defined as the center of the cluster that it belongs to.
However, such hard assignment has issues.
If there are highlihgts in the image, the clustering result will have either have divided patches instead of smooth gradients,
or just simply omit the color change.
See the examples below.
The areas in the red circle are where the problems are.

<div class='art'>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/bulbasaur_to_charizard_kmeans_source.png" alt="Bulbasaur k-means" />
    <div class="caption">K-means result: left is the original image, middle is the reconstructed one, right is the palette</div>
  </div>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/pikachu_to_bulbasaur_kmeans_source.png" alt="Pikachu k-means" />
    <div class="caption">K-means result: left is the original image, middle is the reconstructed one, right is the palette</div>
  </div>
</div>

However, if we represent each pixel as a combination of colors, we will be able to mimic the smooth changes in the image better,
though still not perfect.
Below is a result of weighted color combination.
The highlighted area around Pikachu's neck looks more similar to the original image.

<div class='art'>
  <div class='recoloringpiecewide'>
    <img src="/assets/recoloring/pikachu_to_bulbasaur_blind_separation_source.png" alt="Pikachu weighted combination" />
    <div class="caption">Weighted color combination result: left is the original image, right is the reconstructed one</div>
  </div>
</div>

We formulate the weighted color combination task as an optimization problem.
The goal is to find the parameters (colors as well as wegihts) that minimizes the reconstruction and several other auxilary losses.
The auxiliary losses improves sparsity and diversity.
In particular, every couple of iterations, we snap the color parameters to their closet colors in the image that haven't been assigned yet.
This is to enforce the constraint that we want the extracted colors to be directly from the image.

#### Palette matching


The most important task here is to define mathematically what a "good" transfer is.

### Results
#### Pokemons
#### Art

### Code
Lastly, you can find the code [here](https://github.com/fanyangxyz/pokemon-python).
