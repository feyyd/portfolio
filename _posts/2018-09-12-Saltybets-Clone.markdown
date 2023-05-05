---
layout: post
title:  Saltybets Clone (Pepperstake)
author: Tyler
categories: twitch, c#, php, perl, programming
---

I referred to this project as Iskbet at first, and later changed the name to Pepperstake due to it being a functional clone of saltybets.com.  Upon creation of this project, the intent was to run a Twitch stream that used Eve Online ISK as the currency to bet on mugen matches.  Mugen is a 2d fighting game engine that has the ability to run fights with command line arguments.  Stages, characters (1v1, 2v2), themes, round time, number of rounds, engine speed, and ai toggle can be parameterized via command line.  Because of the potential for mugen to be scripted, and an existing proof of concept for automation (saltybets.com), I chose mugen to be the streaming content that would be wagered upon.  After the betting system was created, the plan was to broadcast other games/matches and allow betting through the same system.

Main page (open in new tab/window to enlarge)-

![Main](../assets/portfolio-images/3-saltybets-clone-mugen-main-page.png)

This site was a fully functional saltybets clone; unfortunately, there is no longer a hosted database, nor do I have a mock database and screenshots from when it was running.  In the top left, there are two columns that fill with all the wagers that were placed on each side of the current fight.  "Player 1:" and "Player 2:" are dummy text that get replaced by the name of the mugen characters being bet on once the round starts.  Most of these table cells are filled or replaced with data via ajax calls to either php pages which call database functions, or ajax calls that load flat text files updated by the administrator pages/controller.  In the center and top right, we have the Twitch interfaces (stream/chat); on bottom left and bottom right we have bet information and balance.  On the bottom center, there are two large buttons labelled Player 1 and Player 2, which are replaced by the current fighting mugen character names.  To bet, the user types the bet amount and clicks on the button with the character name they want to win.  The bar above the buttons and below the Twitch embedded player contained the current match characters, current stage, and the size of the betting pool.  At the bottom right, there is the current mode (Manual (list created by administrators), Random, Random Double, and Paused), and a cell that contained the previous matches winner.

There are some links at the very bottom left that are somewhat hard to see due to the color scheme.  "Get deposit key" would create a key for the user if they didn't have one, and then display it.  When sending money in Eve Online, the user would put this in the Eve mail subject field.  We could then check Eve Online's API for mails periodically and automatically apply the credits to the account with matching deposit key with a script that used the Eve API.  The withdraw form would put a withdraw request into the withdraw table if a valid amount was requested which would be processed by administrators.  "Request New Password" was a form that sent out a password reset link to the registered account's e-mail.  When visiting the site, if a user was not logged in, we would display the login page.  This page contained a login form with user name and password, and a create new account link.  These credentials were then managed with the php sessions.  

Local Controller-

![Local Controller](../assets/portfolio-images/0-saltybets-clone-mugen-controller.png)

On the computer that was streaming mugen, this application would also be ran.  It was first implemented as a perl script, but I re-implemented it as a C# application.  The "Playing?" radio buttons enabled/disabled the main C# program loop.  This controller coordinated mugen and php pages on the web server (ie changing mode whether we click in the application or set the mode via the administrator page).  The basic control flow was as follows: load mugen with next set of characters and AI disabled to show a preview of the upcoming match.  Set the online mode to allow betting; mugen automatically closes after the round's time limit expires.  Set online mode to fighting, disallowing any more bets.  Update online flat files and databases so any users will have updated data via Ajax calls.  Load mugen again, but this time with AI enabled, and now the match has started.  Wait for the round to complete then repeat.

The other UI elements are mostly quality of life/automation of common tasks.  "Set Mugen Path" allowed for running different versions of mugen.  "Load Active Char List" would web query and use the list of characters created by the administrator page, and only run characters from that list.  "Load Full Char List" is similar but it used the full list from the database.  "Load Stage List" would web query and only load stages from database.  "Toggle preview round" would disable the preview round, which was useful for testing. "Toggle Stage/Character Test" would make it so we loaded a local character/stage list instead of using the web query; again, useful for testing.  The Debug button had no functionality, snippets of code could be placed here to execute via button click.  The Update functions allowed for modification of round time, preview round time, and the number of rounds in a match.  The password was needed for any of the web queries to work.  I kept the complexity of this program low by design, offloading a lot of the work to php pages.  I knew the administrator interface would need a lot of the same functionality, so many functions are queries to php pages.

This controller also has the ability to play sounds when the administrator wants to play a sound bytes for the stream.  There is also the ability to read/write to memory of mugen (ie get/set HP).  I also built a command line perl script that could run the AI at high speed to gather statistics, and also initialize the SQL database with the character/stages in the mugen directories.  The commands are as follows:

	mugencontrol observerandom <round_count>            #Fight 2 random characters, output match data
	mugencontrol farmstatsseq <starting_fighter_index>  #Get character statistics sequentially
	mugencontrol farmstatschar <character_abv>          #Get character statistics for single character
    mugencontrol farmstatsrandom                        #Get character statistics in random order
	mugencontrol statsoverall <character_abv>           #Display statistics for character
	mugencontrol statsmatchup <character_abv>           #Display statistics for character matchups
	mugencontrol codegenlist <filename>                 #Generate csv list of characters/stages
	mugencontrol codegenphplist <filename>              #Generate php option list for administrator drop-down menus
	mugencontrol timestoplist                           #Scan logs and generate lists for use with other scripts
	mugencontrol realpercent <character_abv>            #Display win percent for character vs field
	mugencontrol sqlscancharacters                      #Scan character directory and output SQL file for loading to database
	mugencontrol sqlscanstages                          #Scan stages directory and output SQL file for loading to database
	mugencontrol sqllist                                #Generate list of characters to be set active in database

Administrator Page-

![Admin Page](../assets/portfolio-images/1-saltybets-clone-mugen-admin.png)

The administrator page lets a user with proper access control certain functionality of the mugen controller remotely.  At the top, we have the matchup queue and the mode display which are CRUD (Create, Read, Update, Delete) implemented with jquery-ui.  If the mode was set to manual, matches would load from this queue.  If the mode was set to Random Playing or Random Double, whenever a match completed, we would load a new random match based on the character and stage lists.  "Generate Matchup Queue File" would save the current queue to a flat file so that it could be loaded to a cell on the user page.  "Refund bets, Set Mode Paused" would refund the current rounds wagers and pause the application in case of bugs/unintended behaviour.

Admin Views-

![Admin Page](../assets/portfolio-images/2-saltybets-clone-mugen-admin-pages.png)

The above screenshot are the CRUD views implemented with jquery-ui.  Certain updates only allowed selection from pre-defined drop-downs to prevent errors.  Match History stored all prior match data and Stage/Character stored which stages and characters were in the current pool for selection.  In this prototype, the accounts page was exposed to the administrators and would have to be modified once this was moved into a live environment for security reasons.
