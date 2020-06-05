---
title: DIY Microsoft Teams presence led
author: Rogier
type: post
date: 2020-05-06T21:11:56+01:00
url: /2020/05/06/diy-microsoft-teams-presence-led/
commentFolder: 2020-06-05-diy-microsoft-teams-presence-led
categories:
- HomeAutomation
tags:
- ESP32
- ESPHome
- .NET
- MQTT
resources:
- src: animated-microsoft-teams-presence-led.gif
  title: Microsoft Teams presence led
---
Since the outbreak of the Coronavirus (COVID-19) I've been working from home. I have a little daughter (age 5) who also had to stay at home, so more than once I had the same situation as Professor Robert Kelly during a BBC interview:
{{< youtube Mh4f9AYRCZY >}}

Since I recently bought and build a 3D printer ([Prusa i3 MK3S](https://shop.prusa3d.com/en/3d-printers/180-original-prusa-i3-mk3-kit.html)) and watched [Scott Hanselman change his lights](https://www.hanselman.com/blog/MirroringYourPresenceStatusFromTheMicrosoftGraphInTeamsToLIFXOrHueBiasLighting.aspx) during his talk on Microsoft Build 2020, I put 1 and 1 together and decided to try and create something for myself. My goal was to create a led-light that I can put on the wall besides my office door and that changes color following my [Microsoft Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software) presence status (since that's the tool I use for video-calling all day). 

The end result after 3 nights of tinkering looks like this:
{{< figure src="animated-microsoft-teams-presence-led.gif" alt="Microsoft Teams presence led" >}}

The hardware side is made from:
1. [ESP32 DevKit v1](http://s.click.aliexpress.com/e/_dZ9rhiK)
2. [WS2812b RGB-led-ring 12 leds](http://s.click.aliexpress.com/e/_d6X38lU)
3. 9.7 meters of [black PETG filament](http://s.click.aliexpress.com/e/_dXyPbR8) for the base 
4. 10.5 meters of [transparant PETG filament](http://s.click.aliexpress.com/e/_dXhk574) for the shell
5. 3 [female-to-female DuPont jumper wires](http://s.click.aliexpress.com/e/_d9esM4w)
6. (old) phone charger with mini-USB connector (0.7A at 5V)

The ESP32 is programmed with ESPHome software and configured with a relatively easy script, available in my GitHub repo.

The above hardware isn't able to determine the Microsoft Teams presence by itself, it needs a companion app running on a Windows machine. I've created an very minimalistic Universal Windows Platform application using C# which polls the Microsoft Graph API every 15 seconds for the current presence. Based on the response it will sent message to the ESPHome API to change the color of the leds.

Future plans/possible improvements:
- make the light smaller/less high (less printing/less weight)
- downgrade the processor to an ESP8266 (cheaper)
- use less leds, maybe one (?), and test how long this can run on a battery
- add motion sensor to only light up when movement is detected
- reprint shell with different filament to create a diffuser style shell
- improve UI of companion app
- add companion app to Windows traybar instead of a Window
- make color configurable from companion app
- upload companion app to Microsoft Store

Special thanks go out to:
- Scott Hanselman for the initial idea and [blogpost](https://www.hanselman.com/blog/MirroringYourPresenceStatusFromTheMicrosoftGraphInTeamsToLIFXOrHueBiasLighting.aspx)
- Isaac Levin for creating [a similar UWP app](https://github.com/isaacrlevin/PresenceLight?WT.mc_id=-blog-scottha) using off the shelve smart lightbulbs (LIFX and Philips Hue).
- [EzGif.com](https://ezgif.com/) for creating the nice animated gif used in this blog.
- [ESPHome])(https://esphome.io/) for their super easy and powerfull ESP software.
