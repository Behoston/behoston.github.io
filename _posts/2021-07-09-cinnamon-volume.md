---
layout: post
title:  "Linux Mint Cinnamon 20.xx - How to increment volume by 2% (or any %) using media keys"
date:   2021-07-09
categories: PL linux tweaks
header_image : /assets/img/mint_volume/header.jpg
image: /assets/img/mint_volume/header_big.jpg
---

This post contains too much hardware-related content, but hardware is what I like, so here you have: 
**[TLDR; to #How to](#how-to)** 

## Problem

In Cinnamon volume increase (and decrease) step is 5%, Windows has 2% steps, 
Mate has 2% steps, and probably many more environments have the same 2% volume step.
There is no easy way to change this value in Cinnamon
([Windows too](https://social.msdn.microsoft.com/Forums/sqlserver/en-US/fa71e319-f1f2-4382-ad69-6a49e534511a/change-keyboard-volume-increasedecrease-increment-amount?forum=windowspro-audiodevelopment)).


## Why do I need this

For a long time, I've ben using mouse like A4Tech Oscar X-748K (earlier DeLux M555, and another Laser Oscar X-7xx) 
with two programmable buttons under thumb. BTW it's the best fitting mouse I ever use - you should try any of X-7xx.
I always program it to act like media volume+ and volume- it's super handy to me.

![A4Tech X-748K](/assets/img/mint_volume/mouse.jpg)


Some time ago I bought a USB sound card/DAC with a headphone amplifier. 
My previous SMSL M2 have a problem with waking up (noise at 100% volume sometimes for first sound after a long silence).
I found Audioquest Dragonfly Black for about 50 zł on allegro.pl with a damaged jack, easy to fix by replacing the whole jack. 
This one was even easier: resolder jack to pads and align contacts inside the jack, and it works fine :D 
(excluding case, damaged by force opening by me...)

![Audioquest Dragonfly Black](/assets/img/mint_volume/dac.jpg)

This DAC has no volume potentiometer, no buttons. It's raw. Here we have a minor problem: when I use speakers
I can adjust the volume on speakers and tune it using the 5% volume step in Linux, and it's fine and accurate. 
This does not apply to headphones witch has no own volume control, just amplified signal from DAC going directly to headphones.
So, 10% is a little too quiet but 15% is sometimes too loud.

Furthermore, I've got a keyboard (to replace my lovely blue switches :( coz it's too noisy...) 
which has volume wheel above arrow keys - Ajazz K870T. 
It's not easy to change volume only by one step using this roll, and it's easy to swipe it up by accident.
So, increase volume by 10% or even more percent can be painful.

![Ajazz keyboard](/assets/img/mint_volume/keyboard.jpg)

## How to

### Sources

First, you need to enable sources in packet manager.
1. Update Manager
2. Edit
3. Software Sources

![packet manager](/assets/img/mint_volume/packet_manager.png)

Then click OK and go to terminal to download sources of settings daemon.

`xx` - by convention will be any number or prefix/suffix, just check what you have here, 
because it changes in different releases of Cinnamon.


```bash
apt source cinnamon-settings-daemon
cd cinnamon-settings-daemon-xx
```

### Configuration

Edit file `cinnamon-settings-daemon-xx/plugins/media-keys/csd-media-keys-manager.c`
Find line with (should be on top of file):

```C
#define VOLUME_STEP 5           /* percents for one volume button press */
```

and change to 2 or any value you want. **Do not remove `#`**.

### Build

You may need those libraries:
```bash
sudo apt-get install lib32z1 lib32ncurses-dev lib32ncurses6 
```

_If any error appear during the build, try googling it, 90% will be missing dependency._

Ok, lets build and install packet with our adjustment.

```bash
apt build-dep cinnamon-settings-daemon
apt build --no-sign
cd ../
apt deb cinnamon-settings-daemon_xx_amd64.deb
```

Then restart your PC (maybe logout and login will do the job, restarting Cinnamon doesn't seem to work).

Done!

![gif with 2% step](/assets/img/mint_volume/volume_2.gif)
