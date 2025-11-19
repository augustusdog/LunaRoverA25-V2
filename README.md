# LunaRoverA25-V2
LunaRoverA25 had some issues with a. torque, b. steering. LunaRoverA25-V2 aims to overcome these problems

## Introduction
I have a dog named Luna. I sometimes feel bad when I leave Luna to entertain herself. I wanted a cool new project to work on. Considering all that, I present to you.... the LunaRover.

## What already exists?
If I wanted to leave my dog in the garden to entertain herself, what products could I procure to achieve that? And how do those products compare to the real deal of me being with Luna? For example, I can give her a stick - which she will play with, but its nothing like the fun she has when I chase her around my garden. To get an understanding of that I thought it'd be useful to present a venn diagram with buckets for: how I currently entertain my dog, what autonomous systems exist, and what powered vehicles exist. The listed items within each bucket are by no means exhaustive, but inspire idea generation nonetheless.

![Lit Review Venn](./venn.png)


It surprises me that I couldn't find evidence of someone having created an autonomous vehicle to play with their dog... that's something that I've sought to resolve.

## Proposed solution
Brushed motor radio controlled (rc) car modified to drive autonomously with the help of a raspberry pi.

## Method
### Modify rc car to be remotely controlled from a raspberry pi.
#### Step 1 - Understand how the rc car hand-me-down I got worked and retrofitting my own circuitry
I was fortunate enough to be given an rc car for free by my uncle. What's great about the rc car that I got is that it is really easy to understand what is going on, and it meets my torque requirements!

So.... how does it work? A radio transponder sends out a signal to a transceiver (mine was an acoms transceiver). The transceiver then interprets the signal and sends a Pulse Width Modulated (PWM) signal to one of two servos. The first servo controls direction of the front wheels, the second servo controls a mechanical speed controller - which is sort of like a potentiometer for scaling output voltage to the brushed dc motor driving the rear wheels. It's important to note that there is a Battery Eliminator Circuit (BEC) in the acoms receiver which enables the 7.2V from the battery to be stepped down to within the operating voltage range for servo motors.

So, easy right.... well, a bit, but not entirely. My servo motor was a hand-me-down, so naturally I was missing the transponder to control the rc car. So, I had to control the car another way... in the end I opted for a raspberry pi 5 - as I know I'd end up using it for the image detection later on. I stripped out the receiver and modified the circuitry slightly so that it looked like the below:

![Circuit Diagram](./circuit.PNG)

#### Step 2 - Uploading an operating system to the raspberry pi

Without an operating system you're going to have some problems using your raspberry pi.... so definitely don't skip this step. For this step you'll need a microsd card, someway to connect that microsd card to your computer, and [raspberry pi imager software](https://www.raspberrypi.com/software/). Make sure to go to settings and take note of your hostname, user, password, and ensure wi-fi credentials are correct! I had some issues with this step... hopefully you won't.

#### Step 3 - Developing the firmware

Raspberry Pi 5's... just get a 4 and save yourself the hassle. Previously popular GPIO libraries can't be used on the Pi 5, which means you'll have to use another. I used the gpiod library to control my servos - as shown below. In the end I decided to operate my brushed DC motor for X time whenever the up-arrow was pressed, as the motor was far more powerful than I realised and I didn't want to convert that motor speed to more torque with additional gears.

I wrote this code after connecting to my raspberry pi using Secure Socket Shell (SSH) and using the "nano" command.

```
