---
title: Installing OS on Cubietruck and SSD
author: rogier
type: post
date: 2014-04-27T20:33:55+02:00
url: /2014/04/27/installing-os-on-cubietruck-and-ssd/
commentFolder: 2014-04-27-installing-os-on-cubietruck-and-ssd
categories:
- HomeAutomation
tags:
- Cubieboard3
- Lubuntu
resources:
- src: PhoenixSuite-flashing.png
  title: PhoenixSuite-flashing
- src: PhoenixSuite-done.png
  title: PhoenixSuite-done
- src: Lubuntu-need-upgrade.png
  title: Lubuntu-need-upgrade
aliases:
- index.php/2014/04/installing-os-on-cubietruck-and-ssd/
---
Based on [Robert Hekkers experience with installing the Cubieboard3](http://blog.hekkers.net/2013/12/01/meet-the-cubietruck-aka-cubieboard3/), I downloaded the latest Lubuntu server OS v1.02 image: [http://docs.cubieboard.org/tutorials/a20-cubietruck_lubuntu_server_releases](http://docs.cubieboard.org/tutorials/a20-cubietruck_lubuntu_server_releases)

I downloaded and installed the latest Phoenix Suite v1.06 for Windows using this download: [http://docs.cubieboard.org/tutorials/common/livesuit_installation_guide](http://docs.cubieboard.org/tutorials/common/livesuit_installation_guide)
After starting the Phoenix Suite, I was asked if I wanted to install the upgrade to v1.08. I clicked “yes”. (btw, DON’T install this software on a VM-ware virtual machine, I couldn’t get a stable connection to the Cubieboard3 and as a result the flashing would not start)

Then I followed this guide to install Lubuntu on the Cubietruck: [Cb3 Lubuntu-12.10-desktop Nand Installation V1.00](http://docs.cubieboard.org/tutorials/ct1/installation/cb3_lubuntu-12.10-desktop_nand_installation_v1.00)

[{{< img "PhoenixSuite-flashing.png" ""  "PhoenixSuite-flashing" >}}](https://www.progz.nl/homeautomation/wp-content/uploads/sites/2/2014/04/PhoenixSuite-flashing.png)

After 4 minutes and 25 seconds Lubuntu was flashed to the Cubietruck and was up-and-running:

{{< img "PhoenixSuite-done.png" ""  "PhoenixSuite-done" >}}

I downloaded [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) and connected to the ip-address of the Cubieboard3 (looked that up in my router, since the Lubuntu is configured to use DHCP). The default user that is created is called “linaro” and the password is also “linaro”.

Because the onboard NAND-flash has limited performance and a limited life-cycle I moved the OS from the onboard NAND-flash-chips to a new Samsung 840EVO SSD, using this Youtube-video as a guide: [Flashing LUbuntu Server to a Cubietruck and Moving It to a Hard Drive](http://www.youtube.com/watch?v=2SA-6Wj_kN0&list=PLzRTHShWI-00KGTbUUUSCaKC7coNdArme&feature=share&index=2)

When I logged back into the Cubietruck I saw that the Lubuntu version was out-dated:

{{< img "Lubuntu-need-upgrade.png" ""  "Lubuntu-need-upgrade" >}}

To upgrade it to the latest version I used the following commands (found at a blog called [How to install Lubuntu Server on Cubietruck from Mac OS X](http://dyhr.com/2013/11/22/how-to-install-lubuntu-server-on-cubietruck-from-mac-os-x/)):


* `# apt-get update; apt-get upgrade`
* `# apt-get install python-apt`
* `# do-release-upgrade`  
  The last command took about a half hour to complete. Then I finished the rest of the customizations steps, mainly:
* Adjusting time-zone  
`#rm /etc/localtime`  
`#ln -s /user/share/zoneinfo/Europe/Amsterdam /etc/localtime`  
`#nano /etc/timezone`  
Changed to “Europe/Amsterdam”
* Adding a new user  
`#adduser <username>`  
`#sudo adduser <username> sudo`  
`#sudo usermod -a -G sudo <username>`  
* Removing the default linaro user  
`#sudo userdel -r linaro`
* Disable apache auto-start (I will be using Node.js instead)  
`#sudo update-rc.d -f apache2 remove`
* Install Node.js and it’s package manager NPM  
`#apt-get install nodejs`  
`#apt-get install npm`
* Install monit  
`#apt-get install monit`
* Install aptitude and ntp  
`#apt-get install aptitude`  
`#apt-get install ntp`

The final step will be to place the Cubietruck in the [Ewell case](http://cubieboard.org/2014/02/27/ewell-has-come-minipc-not-be-far-behind/) but first I must figure out how to connect a NRF24L01+ board and fit that into the case as well.
