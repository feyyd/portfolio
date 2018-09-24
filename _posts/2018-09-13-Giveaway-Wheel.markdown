---
layout: post
title:  Giveaway Wheel
author: Feyd
categories: twitch, eve, c#, unity, programming
---
<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/ZnCCFtpPYkM?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>
<p />

This is a giveaway wheel created in Unity to facilitate giveaways on Twitch.tv.  The wheel fits snugly up against the top left corner to block certain Eve UI elements, and it is chroma keyed so that there is a transparency effect when used with OBS (Open Broadcaster Software).

Chroma Key-

![Chroma Key](../assets/portfolio-images/0-wheel.png)

Clicking the spin button plays music and spins the central ship.  After the spin is complete, the prize that was won is indicated by making it pulsate.  In the top left, the prize that was won on the last roll is saved; I was missing which prize was won on the previous spin. There is a random number generator function to do ISK giveaways, and there are hotkeys to play sound clips for the stream.  There are a variety of different ships that can be spun.  One can also add more ships in Unity, given one has the models and textures.  There are a few camera pathing functions, but they do not see much use on stream.


