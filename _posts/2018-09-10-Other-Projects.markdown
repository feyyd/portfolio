---
layout: post
title:  Other Projects
author: Feyd
categories: twitch, c#, php, perl, programming
---
<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/E5b7X1H8G0Q?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>
<p />

Twitch Space Game - A Unity application for Twitch that uses my Particle Bullets Unity asset.  Users could join with !join in Twitch chat, and buy upgrades at end of round with !buy.  User messages from Twitch chat could be displayed above their ships.  This program had IRC integration so the streamer could interact with chat from within the application.  After the join period, users would be put into the game field; their ships shot in a simple pattern similar to the old-school hockey/football shakers.  A random direction is chosen every few seconds, and bullets spray at random intervals.  Players would get score based on kills of npcs or other players, and they could upgrade their ship with the points.  It is currently in a non-functional state due to Twitch authorization changes, but above is a video of the basic game loop.

Proc Killer - A C# Windows tray application that kills processes and keeps count.  I created this application to automatically kill error processes that would stack up with the game Rift and cause major slowdown.

Magic: The Gathering Sample Draws - A PHP program that takes an apprentice deck (common format for M:tG decks) and number of rows as input, then outputs a row of 10 randomly selected cards, without replacement, to simulate an initial hand (7 cards) and the next 3 draws.  The program outputs a newly randomized 10 cards based on the number of rows input from the query string and outputs them into an HTML table.

Everquest DPS Calculator - A custom damage (dps) calculator written in perl for Shadow Knights.

Farkle Simulator - A dice game simulator for Farkle on Facebook written in perl.

Powerball Simulator - Simple C tool to run billions of Powerball draws and track wins.
