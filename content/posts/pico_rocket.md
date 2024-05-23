+++
title = 'Pico Powered Rocket'
date = 2024-04-20T16:19:54-05:00
draft = true

tags = ['raspberry pi', 'circuitpython', 'pi pico']

[cover]
image = "https://assets.raspberrypi.com/static/74679d6c81ffc5503a20b64feae2ed4f/2b8d7/pico-rp2040.webp"
alt = "The Pi Pico Microcontroller"
relative = false
+++

Last summer, me and one of my friends thought it would be cool if we built a model rocket with a payload (Sensors, Camera, Lego Minifig, etc.). So, with the help of another of my friends, we got to work. We decided we wanted our payload to be a microcontroller with Adafruit's [BME280](https://www.adafruit.com/product/2652) and [LSM6DSOX+LIS3MDL](https://www.adafruit.com/product/4517) breakout boards.

The original plan was to use Raspberry Pi's new microcontroller called the Pico (seriously, it is *tiny*!). I already had one from an auto-clicker project. The only problem 