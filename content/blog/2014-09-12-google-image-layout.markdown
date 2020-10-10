---
layout: post
title: Google Image Layout
date: 2014-09-12 21:46:14.000000000 +12:00
---
Recently, my project required me to implement an image gallery which has the layout look like Google Image Search, Flickr Explore, Google Plus Photo Gallery. They have a really nice looking gallery, with images aligned without croping, re-ordering, and the images scale by its ratio.

I wonder how they did that, and after some investigation on the internet, i found a very nice solution. Which i will share with you here.

[LIVE DEMO](http://ptgamr.github.io/google-image-layout/)

##The Algorithm

###Initial Images
![initial](http://i.imgur.com/9d31ohG.png)

Let say we have 3 images, width size `w1xh1`, `w2xh2`, `w3xh3` corespondingly. We want to align them in one line.

###Expected Alignment
![Expected](http://i.imgur.com/3Krnjy5.png)

The `container width` is known, now we have to find w1'xh1', w2'xh2', w3'xh3'.

The trick is: `all the images of the same row are have the same height.`

Which mean `h1' = h2' = h3'`

Appling some basic maths lead us to the Formular

###The Formular
![The Formular](http://i.imgur.com/IMgadeK.png)

###How many images should fit in one row?
![How many images?](http://i.imgur.com/USNb3s9.png)

Imagine that we have `n` images. We have to define a threshold `maxHeight` for the height, says i dont' want any of the row has the height more than that. What we have to do then is to make the choice:

- start `x = 1`
- Extract `x` images from the array
- calculate `h`
- if `h < maxHeight` then put those images into the row
- otherwise, increase counting `x = x + 1`, then start over again

##See it in action

[http://ptgamr.github.io/google-image-layout/](http://ptgamr.github.io/google-image-layout/)

I've implemented a small library on [my github account](https://github.com/ptgamr/google-image-layout) to help you achive this easily. If you want to understand more about the implementation, pls take a look into the code. If you have any suggestion, don't hesitate writing me a comment.
