---
title: Home-automation foundation
author: rogier
type: post
date: 2014-04-27T15:53:02+02:00
url: /2014/04/27/home-automation-foundation/
commentFolder: 2014-04-27-home-automation-foundation
categories:
- HomeAutomation
tags: []
resources:
- src: Cubietruck.png
  title: Cubietruck
aliases:
- index.php/2014/04/home-automation-foundation/
---
In the [blog of Robert Hekkers](http://blog.hekkers.net/) he writes he started out using a PC running a single monolithic app developed in Delphi. He recently completed porting all his software from a PC to an ARM-based-board named [Cubieboard3](http://docs.cubieboard.org/products/start#cubietruck_cubieboard3) (aka Cubietruck). In my house there is a PC running 24/7 as a NAS, but I would like to keep things separate so I’m also going to use a Cubieboard.

[{{< img "Cubietruck.png" "Cubieboard 3 aka Cubietruck"  "Cubietruck" >}}](https://www.progz.nl/homeautomation/wp-content/uploads/sites/2/2014/04/Cubietruck.png)

Robert Hekkers also switched his programming language from Delphi to [Node.js](http://nodejs.org/) when moving over to the Cubieboard. Since I don’t have any experience with Node.js but would love to learn, I see this as a great opportunity to start using something new, so I’m going to follow his example.

The Cubietruck will function as the central node in the whole home-automation system. In all the rooms there will be several sensors/controls and my main idea right now is to use Arduino based controllers for the sensor-nodes. Since not all rooms have Ethernet and I don’t think a sensor should have WiFi, I’ve selected a radio-chip called the [nRF24L01+](http://www.nordicsemi.com/eng/Products/2.4GHz-RF/nRF24L01P) as my main communication-device between the sensors and the central-node. ManiacBug has done great work building [battery-powered sensor-nodes](http://maniacbug.wordpress.com/2011/10/19/sensor-node/) and writing an [Arduino library](https://github.com/maniacbug/RF24Network) to transport the sensor-data to the central-node. This actually comes pretty close to what I would like to build, only I would like to add more sensors, create interaction based on sensor-values and add a nice front-end (website/mobile app) to control everything.
