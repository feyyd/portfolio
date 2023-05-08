---
layout: post
title:  Treasure Draft (unet)
author: feyd
categories: unity, unet, c#, programming
---
Treasure Draft is a multi-player card game I built for my associate.  It is a draft based game where players select cards with point values and effects.  After all cards are selected, the score is tallied to determine a winner.  We moved on to Tabletop Simulator for testing/creation as it was much more rapid development, but this is the original prototype.

<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/he7qryfnbgs?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>
<p />

To start a game, one must first create a game with the matchmaker or by clicking the "Play and Host" button in the lobby.  This application assumes the host is also a client, so it does not work with dedicated server option.  The lobby and game scene are separate, so we must pass any data from lobby to game scene.  In this prototype, there is just a name and color sent to the game scene, but any pre-game setup information or values can be added for pass along easily.  After a game has been created, users can enter their name and color; when at least one other player joins the pre-game lobby, and all players have selected join, the game will launch.

Unity's networking scheme (unet) has both client and server code in the same executable.  This program stores shared data on the host in synchronized lists (SyncList).  Clients can execute server code with Commands, and the host can execute code on clients remotely with ClientRpc.  One can also register a callback to a SyncList, and when the list is changed on the host, each client's callback will execute.

The game starts by displaying some cards that can be drafted.  Certain cards have effects and go into play, while other cards are only worth points and go to a face down pile.  The count we see by each player is the number of cards that do not have effects.  Cards that have effects do not add to the count; they are added to an in play zone which is not displayed to the clients in this prototype, but are known to the server.  A player is selected at random to select a card first.  After the first player selects a card, effects are applied and the next player will be allowed to pick.  The amount of packs each player has can be set; in the video, each player has one pack.  We see each player alternate picking cards that get removed from the current pack.  Once all cards are gone, a new pack is generated, and the process repeats until all packs are depleted.  After all cards have been selected and no packs remain, the score is tallied.  The player with the highest score is the winner.

The animations in this prototype are synced with ClientRpcs that advance each animation's state machine.  Each client checks for mouse clicks, and if they are the active player, a Command is sent to the host to remove the card that was clicked.  The client sent Command updates SyncLists on the host, then the host sends a ClientRpc back to all clients to update their local gamestate to match that of the host.  Each client has an event log that gets updated via ClientRpc calls from the host.  Each client informs this event log of its own events by sending a Command with an event type so we know what to log.  Chat in this program displays above the user's name, but we could also implement a rolling chat similar to the event log if it were desired.

<div class="videoWrapper">
<iframe width="560" height="315" src="https://www.youtube.com/embed/c77-KKiy7d8?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe></div>
<p />

This is a stub project using unet.  It is a starting point for unet projects that covers basic multi-player functionality that can be extended.  It has player movement, mouse click spawning of network objects, and a simple network lobby.
