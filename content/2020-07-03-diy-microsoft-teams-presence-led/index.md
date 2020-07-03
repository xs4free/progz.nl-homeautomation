---
title: DIY Microsoft Teams presence led
author: Rogier
type: post
date: 2020-07-03T11:11:56+01:00
url: /2020/07/03/diy-microsoft-teams-presence-led/
commentFolder: 2020-07-03-diy-microsoft-teams-presence-led
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
- src: diy-microsoft-teams-presence-led-3d-case.jpg
  title: Microsoft Teams presence led 3D case
- src: teams-presence-publisher-screenshot.jpg
  title: Microsoft Teams Presence Publisher screenshot  
---
Since the outbreak of the Coronavirus (COVID-19) I've been working from home. I have a little daughter (age 5) who also had to stay home from school, so more than once I had the same situation as Professor Robert Kelly during a BBC interview:
{{< youtube Mh4f9AYRCZY >}}

Since I recently bought and build a 3D printer ([Prusa i3 MK3S](https://shop.prusa3d.com/en/3d-printers/180-original-prusa-i3-mk3-kit.html)) and watched [Scott Hanselman change his lights](https://www.hanselman.com/blog/MirroringYourPresenceStatusFromTheMicrosoftGraphInTeamsToLIFXOrHueBiasLighting.aspx) during his talk on Microsoft Build 2020, I put 1 and 1 together and decided to try and create something for myself/daughter. My goal was to create a led-light, that I can put on the wall besides my office door, that changes color following my [Microsoft Teams](https://www.microsoft.com/microsoft-365/microsoft-teams/group-chat-software) presence status (since that's the tool I use for meetings/video-calls all day). 

The end result after 3 nights of tinkering looks like this:
{{< figure src="animated-microsoft-teams-presence-led.gif" alt="Microsoft Teams presence led" >}}

The hardware side is made from:

| Part name | Cost (incl. shipping) |
| :---      | ---: |
| [ESP32 DevKit v1](http://s.click.aliexpress.com/e/_dZ9rhiK) | €4,33 |
| [X-Ring WS2812b RGB-led-ring 12 leds](https://www.hobbyelectronica.nl/product/x-ring-12-bits-ws2812b-rgb-led-ring/) or [this on AliExpress](http://s.click.aliexpress.com/e/_d6X38lU)| €2,35 |
| 9.7m of [black PETG filament](http://s.click.aliexpress.com/e/_dXyPbR8) for the base (€19,92 for 300m) | €0.65 |
| 10.5m of [transparant PETG filament](http://s.click.aliexpress.com/e/_dXhk574) for the shell (€19.52 for 300m) | €0.68 |
| 3 [female-to-female DuPont jumper wires](http://s.click.aliexpress.com/e/_d9esM4w) (€1.09 for 40pcs)| €0.08 |
| (old) phone charger with mini-USB connector (0.7A at 5V) | - |
| | Total: **€8,09**

{{< figure src="diy-microsoft-teams-presence-led-3d-case.jpg" alt="Microsoft Teams presence led 3D case" >}}

The filament is used to print a custom designed case, which consists of 2 pieces that hold the ESP32 and the RGB-led-ring. They slide nicely over each other and lock together at the USB opening. The case can be viewed/downloaded at [Autodesk Fusion 360 - Team Presence Light](https://a360.co/3gYzPql) or [Thingiverse - Teams Presence Led](https://www.thingiverse.com/thing:4434525).

The software on the ESP32 is [ESPHome](https://esphome.io/) and configured with a relatively easy script:
```
esphome:
  name: teams_presence_led
  platform: ESP32
  board: esp32doit-devkit-v1
  on_boot:
    then:
      - light.turn_off: xringid

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  ap:
    ssid: "Teams Presence Led"
    password: !secret wifi_hotspot_password

captive_portal:

logger:

api:

ota:

web_server:
  port: 80

light:
  - platform: neopixelbus
    type: RGB
    pin: GPIO26 #D26
    num_leds: 12
    variant: WS2812
    method: ESP32_I2S_1
    name: "xringname"    
    id: "xringid"
```

The script is also available on [my GitHub repo for this project](https://github.com/xs4free/MicrosoftTeamsPresenceLed).

The above hardware isn't able to determine the Microsoft Teams presence by itself, it needs a companion app running on a Windows machine. I've created an very minimalistic Windows application using C# and WPF which polls the Microsoft Graph API every 15 seconds for the presence of the signed in user. Based on the presence it will sent a message to the ESPHome API to change the color of the leds.

{{< figure src="teams-presence-publisher-screenshot.jpg" alt="Microsoft Teams Presence Publisher screenshot" >}}

The app also has the ability to publish your Microsoft Teams status to MQTT. I haven't extensively tested this feature, but I'm going to use it in the future with Home Assistant to also light up the led at night, as a night-light for my daughter.

If you want to re-create this project, head on over to my [GitHub repo MicrosoftTeamsPresenceLed](https://github.com/xs4free/MicrosoftTeamsPresenceLed). There you can find all source-code, stl-files and the configuration script I used.

Future plans/possible improvements:
- make the case smaller/less high (less printing/less weight)
- try to downgrade the processor to an ESP8266 (cheaper)
- use less leds, maybe one (?), and test how long this can run on a battery
- add motion sensor to only light up when movement is detected
- reprint shell with different filament/settings to create a diffuser style shell
- improve UI of companion app
- make color configurable from companion app
- [and more...](https://github.com/xs4free/MicrosoftTeamsPresenceLed/issues)

Special thanks go out to:
- Scott Hanselman for sparking the initial idea and his [blogpost](https://www.hanselman.com/blog/MirroringYourPresenceStatusFromTheMicrosoftGraphInTeamsToLIFXOrHueBiasLighting.aspx)
- Isaac Levin for creating [a very similar UWP app](https://github.com/isaacrlevin/PresenceLight?WT.mc_id=-blog-scottha) using off the shelve smart lightbulbs (LIFX and Philips Hue).
- [ESPHome](https://esphome.io/) for their super easy and powerfull ESP software.
- [EzGif.com](https://ezgif.com/) for creating the nice animated gif used in this blog.
