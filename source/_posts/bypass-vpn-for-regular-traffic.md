---
title: Bypass VPN for regular traffic
tags:
  - Vista
  - VPN
  - xp
url: 314.html
id: 314
categories:
  - Did you know
date: 2008-08-26 04:08:24
---

![IMG_1380](/uploads/2008/08/img-1380.jpg) For as many places as I've been where they use VPNs, I've yet to find one that is set up correctly.  I suppose there is a good reason for this, but I consider the problem to be mostly Microsoft's fault. I mean, wouldn't you assume that if it were possible to use your regular connection for all the network traffic EXCEPT for the traffic that needs to go through the VPN, that is what you would want?  But no.  Microsoft sets it up so that ALL of your traffic goes through the VPN connection.

<!-- more -->

What this means is that getting a connection to a search engine in order to look for a solution to a problem will take about twice as long as it should since your traffic first has to go to the VPN server and then out to the search engine. Here's how you fix it:

## In Vista:

Go into the Control Panel and click the "Network and Sharing Center" icon. On the left panel of the resulting screen you should see a link, "Manage network connections."  Click it.

The next screen will have icons for all of your connections.  There should be one for your VPN.  Right-click it and select "Properties" from the menu.

In the "Properties" screen, click the "Networking" tab and then select "Internet Protocol Version 4" and click the "Properties" button.

Click the "Advanced" button.  This will bring up a new window where you can un-check "Use default gateway on remote network." OK out to save everything.

## In XP:

Go into the Control Panel and  click "Network Connections" Right click the icon for the VPN and select "Properties" from the menu.

In the "Properties" screen, click the "Networking" tab and select "Internet Protocol" from the list and click the "Properties" button.

On the window that pops up, click the "Advanced" button. Un-check the "Use default gateway on remote network" check box.

## What this does:

Now the only traffic that will go to the VPN is traffic bound for the VPN on the same subnet as the subnet the VPN connection is on. If you need other traffic to also go through the VPN, you'll need to play with the routing tables.  
