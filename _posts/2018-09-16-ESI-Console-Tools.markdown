---
title: ESI Console Tools
author: feyd
layout: post
categories: eve, c#, programming
---

Undercutting is when someone offers to pay a higher amount on a buy order, or offers to sell something cheaper on a sell order.  "Undercut Check" is a console ESI (Eve Swagger Interface) application for Eve Online that is written in C#.  It checks market orders of a character or corporation and returns all orders that have been undercut.  This application saves on the amount of times a user has to click when updating orders on the market.  I bundled multiple tools into this solution to reuse the authentication and command line options functionality.  It also has the ability to check ship insurance and output fleet member information.  When authentication is needed, the default browser is launched with a generated OAuth (Open Authentication) URL.  The application then creates a lightweight HTTP server to handle the response that comes after logging in with an Eve Online account.  After it receives a response from authentication, the server is shut down, and necessary tokens are saved for reuse.  

Options-

![Options](../assets/portfolio-images/0-consoletool-help.png)

Undercut Check-

![Market](../assets/portfolio-images/1-consoletool-undercut.png)

Data Source is used for all functionality (market, insurance, fleet inspect). Region (-r), Price Threshold (-p), Order Type (-t), Corporation (-c), and Character (-ch) are used for what this application was originally intended for: Undercut Checking.  One can lookup orders by character or corporation, show buy and/or sell orders, set a price threshold for which orders to see, and set which region to check prices against.

Insurance-

![Insurance](../assets/portfolio-images/2-consoletool-insurance.png)

Using the -i flag followed by a ship name will show platinum insurance information for the given ship, and "-i all" will show platinum insurance for all ships.  Ship names are loaded from the most recent static data export.  Also displayed is the difference between platinum insurance payout and its cost, which is the value players usually want when evaluating insurance decisions.

Fleet Inspection-

![Fleet Inspection](../assets/portfolio-images/3-consoletool-fleet-inspect.png)

The -f flag followed by character name will grab all fleet member information of the fleet the named character is in.  The ship name is shown at the start of each fleet line, which is loaded from the most recent static data export.
