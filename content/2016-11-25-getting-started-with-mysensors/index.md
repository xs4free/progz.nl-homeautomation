---
title: Getting started with MySensors
author: rogier
type: post
date: 2016-11-25T21:09:56+01:00
url: /2016/11/25/getting-started-with-mysensors/
commentFolder: 2016-11-25-getting-started-with-mysensors
categories:
- HomeAutomation
tags:
- MySensors
resources:
- src: MySensors-Getting-Started-Humidity-Node-1024x768.jpg
  title: MySensors humidity node
- src: MyNodesNET-first-node.png
  title: MyNodes.NET humidity node
aliases:
- index.php/2016/11/25/getting-started-with-mysensors/
---
For a long time I have been following the website/project [MySensors.org](https://www.mysensors.org/). I've been thinking about building a temperature and humidity sensor-network in my house for a long time and this project seems to have figured out all the hard stuff (radio communication, low-power battery sensors, network gateway and even controllers).

The [Getting Started](https://www.mysensors.org/about) guide on MySensors.org is a really great starting point to read about the MySensors project and see what it can do.

<span lang="en-US">For my first project I used the following hardware</span><span lang="nl">:</span>


1.  [ESP8266 NodeMcu v0.9](https://www.aliexpress.com/item/V3-Wireless-module-NodeMcu-4M-bytes-Lua-WIFI-Internet-of-Things-development-board-based-ESP8266-for/32532972941.html)
2.  [Arduino Pro Mini 3.3v (ATMEGA328 at 8 MHz)](https://www.aliexpress.com/item/1pcs-lot-Pro-Mini-328-Mini-3-3V-8M-ATMEGA328-ATMEGA328P-AU-3-3V-8MHz-for/32313595044.html)
3.  [<span lang="nl">NRF24L01+</span>](https://www.aliexpress.com/item/1PCS-NRF24L01-NRF24L01-Wireless-Module-2-4G-Wireless-Communication-Module-Upgrade-Module/32667467720.html)<span lang="en-US"> (2 pieces)</span>
4.  [AM2302/DHT22](https://www.aliexpress.com/item/1pcs-DHT22-digital-temperature-and-humidity-sensor-Temperature-and-humidity-module-AM2302-replace-SHT11-SHT15/32316036161.html)
5.  [FTDI USB 3.3v programmer](https://www.aliexpress.com/item/1pcs-FT232RL-FTDI-USB-3-3V-5-5V-to-TTL-Serial-Adapter-Module-forArduino-Mini-Port/32650148276.html)
<span lang="en-US">I setup the </span>[<span lang="en-US">ESP8266 as an WiFi gateway</span>](https://www.mysensors.org/build/esp8266_gateway)<span lang="en-US">, the only thing I added was an extra 22</span><span lang="nl">µF </span><span lang="en-US">capacitor between the ground and the power of the NRF24L01 as described here under </span>[<span lang="en-US">Connecting a Decoupling-Capacitor</span>](https://www.mysensors.org/build/connect_radio)<span lang="en-US">.</span>


With the Arduino Pro Mini I created a [temperature and humidity sensing node](https://www.mysensors.org/build/humidity). When I started building this node, the MySensors-website contained an [old version of the code](https://github.com/mysensors/MySensorsArduinoExamples/blob/324679971fcefaa02cbe02b54f7d07a0209c2ccd/examples/HumiditySensor/HumiditySensor.ino) that was not compatible with the latest (2.0) version of the MySensors-library. So I ended up changing the code and fixing a couple of problems. I was planning on writing down all the steps that I took, until I discovered that Henrik Ekblad created a new version of the code with the exact same fixes. So if you go to [Air Humidity Sensor](https://www.mysensors.org/build/humidity) page right now, you will have a working node in minutes (mine took an evening of tinkering).[{{< img "MySensors-Getting-Started-Humidity-Node-1024x768.jpg" ""  "MySensors humidity node" >}}](https://www.progz.nl/homeautomation/wp-content/uploads/2016/11/MySensors-Getting-Started-Humidity-Node.jpg)

After creating the gateway and the humidity-node, I needed a (what MySensors calls) [Controller](https://www.mysensors.org/controller). Since I'm doing all my work on a Windows machine and my day-job is a .NET Developer, I went for [MyNodes.NET](http://www.mynodes.net/). After starting the software (with localhost.cmd run as administrator) and opened a browser to [http://localhost:1312](http://localhost:1312), all I needed to do was add the ip-address of the gateway in the configuration-section and when it was connected, the temperature&humidity node appeared!

[{{< img "MyNodesNET-first-node.png" ""  "MyNodes.NET humidity node" >}}](https://www.progz.nl/homeautomation/wp-content/uploads/2016/11/MyNodesNET-first-node.png)

This was just my first step at creating a sensor network for my house. It only proves that the MySensors library is a very good fit for the sensors I'm planning on creating. The next step will be to convert the gateway to a MQTT gateway and use a different controller that supports more hardware and has a larger (more active) community (most likely it's going to be [Domoticz](http://domoticz.com/)).
