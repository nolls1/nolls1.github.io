---
layout: post
title: Raspberry Pi RC Truck
---

# INTRODUCTION

I ordered my Raspberry Pi back in May 2015, and after looking up various ways to utilize it I decided to use it to hack an RC Truck so that it can be driven from a smartphone. This post serves as a general guide to show you how I modified the RC Truck, so feel free to use the code I have for it on my GitHub, or feel free to ask questions by emailing me at [crnoller1@gmail.com](mailto:crnoller1@gmail.com). In addition the [this instructable](http://www.instructables.com/id/Toy-Truck-Powered-by-Raspberry-Pi/?ALLSTEPS) served as my main reference for whenever I got stuck:

The following is a total list of materials I used to build the project:

- [Raspberry Pi 2 Starter Kit](http://www.amazon.com/gp/product/B008XVAVAW?psc=1&redirect=true&ref_=oh_aui_detailpage_o02_s00)
- [Maisto RC Truck](http://www.amazon.com/gp/product/B00Y53XH9O?psc=1&redirect=true&ref_=oh_aui_detailpage_o00_s00)
- [Adafruit Stepper Motor Hat](http://www.amazon.com/gp/product/B00TIY5JM8?psc=1&redirect=true&ref_=oh_aui_detailpage_o09_s01)
- [Power Bank for the Pi](http://www.amazon.com/gp/product/B00JM59JPG?psc=1&redirect=true&ref_=oh_aui_detailpage_o09_s00)
- [Standoff Screw/Nut Kit](http://www.amazon.com/gp/product/B018C19KOA?psc=1&redirect=true&ref_=oh_aui_detailpage_o08_s00)
- [Double Sided Tape](http://www.amazon.com/Scotch-Exterior-Mounting-1-Inch-60-Inch/dp/B00004Z4BV/ref=sr_1_9?ie=UTF8&qid=1456782720&sr=8-9&keywords=double+sided+tape)
- Breadboard Wires (any breadboard wire kit on amazon will be fine)

# DISASSEMBLY

The first thing to do was to take apart the truck to get rid of anything that wasn't relevant anymore. When I first got the truck it looked like this:

![Truck1](/images/Truck1.JPG)

After taking the top off of it and opening up the housing that holds the onboard chip for the truck it looked like this:

![Truck2](/images/Truck2.JPG)

Now in this picture you can somewhat see how the wires that are soldered to the chip serve their purpose. The wires going from the chip to the axles will power the motors, and the wires going from the chip that disappear under it will be for the batteries and the radio communication for the remote. At this point, I labeled what each wire did which is shown in this picture:

![Truck3](/images/Truck3.JPG)

After labeling the wires, I unsoldered them from the chip so I could take the chip out. The chip isn't relevant and it isn't needed from this point forward. I also removed the chip that controlled the frequencies for the remote that came with the truck. At this point the only wires that I needed were:

- The power and ground for the batteries that power the motors
- The power and ground for each motor that drives the wheels
- The power and ground for the motor in the front axle that controls the steering

So I double checked to see if they were labeled properly and set them to the side. If you need reassurance you can hook the motors up to a battery and see if they still work at this point. The next thing to do is to assemble the Adafruit Stepper Motor Hat and put it on the Pi.

# ASSEMBLE STEPPER MOTOR HAT AND PI

The Raspberry Pi is limited in regards to its PWM capabilities, so that is where the stepper motor hat comes in. It has 4 terminal blocks that will allow you to control up to 4 DC motors with the Pi. As Adafruit advertises, you can actually stack multiple hats on top of each other to let you control more than 4 DC motors. The stepper motor hat requires that you solder on the terminals blocks and the header that will plug into the Pi's GPIO pins. After soldering it together you can stack it on top of the Pi. After I put them together it looked like this:

![Truck4](/images/Truck4.JPG)

In the picture you can see that I used wire-ties as a makeshift standoff. I only did that, because I was waiting for the standoff kit in the mail. I do not recommend you use wire-ties as standoffs as they proved to be really unstable. Once it was all put together I had to set up the hat so it could communicate with the Pi. The way it does it is over I2C, which is a serial bus that the Pi can use to communicate with external devices. To set up the hat I refered to [Adafruit's website](https://learn.adafruit.com/adafruit-dc-and-stepper-motor-hat-for-raspberry-pi/installing-software). After I got the hat to successfully communicate with the Pi, I was ready to put the Pi and hat on the truck.

# HOOK UP PI AND POWER BANK TO RC TRUCK

What I did first was I layed the Pi and the power bank on the platform of the truck, and I shifted things around to get an idea of how to mount it. It just so happened that the power bank fit perfectly on the platform, so I just mounted the pi on top of it. I mounted all of it using double-sided tape, which proved to be more stable that I thought. The power bank I bought was rated for 10000mAh which is kind of overkill, so you can probably get away with a smaller one that is rated for ~2000mAh or something, but just make sure it is a 5 volt battery since that is what the Pi needs. The next step was to wire up the motors and battery to the corresponding terminal blocks on the stepper motor hat. The terminal blocks on the stepper motor hat are labeled M1, M2, M3, and M4, and the terminal block for the batteries to power the motors is set off to the right of the other 4 terminals. This is shown in the following picture:

![Truck5](/images/Truck5.JPG)

At this point there is no additional construction needed, so now all it needed was the software.

# WRITING THE SOFTWARE

What needed to happen was that I had to write the code to power the motors and design the web application which would let me control the truck. The type of code I needed to write was python for the motors and hosting the webpage, and I needed to write some HTML/CSS for the button layout that would be hosted with the web framework I would be using, which was flask. The stepper motor hat made everything pretty straightforward, since it has libraries that make it easy to write the code for the motors. To get started I refered again to [Adafruit's website](https://learn.adafruit.com/adafruit-dc-and-stepper-motor-hat-for-raspberry-pi/using-dc-motors):

Once I figured that out I needed to make a button layout that would be easy for me to use. Since I don't really know that much about HTML/CSS I consulted with my friend, Daniel Forsyth, to help me out with it since that is in his realm of expertise. If you're interested, his blog can be found [here](http://www.danielforsyth.me/). The following picture is the button layout:

![Layout](/images/Layout.PNG)

After having the code for the motors and the button layout, I used flask to host the web page that the button template would be displayed on. I refered to [Miguel Grinberg's Blog](http://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) for flask information.

After getting all of the code working, I went and tested it out and it was a success. If you want to take a look at the code [here is the repo](https://github.com/nolls1/RC_Truck). The final product looks like this:

![Truck6](/images/Truck6.JPG)

Here is a video I took of me testing it out. There is a slight delay when hitting the buttons to tell the truck where to go, but altogether it turned out better than I thought it would (click the image below for the youtube video):

<a href="http://www.youtube.com/watch?feature=player_embedded&v=BZwULtBIjSY
" target="_blank"><img src="http://img.youtube.com/vi/BZwULtBIjSY/0.jpg" 
alt="RC Truck Video" width="480" height="360" border="10" /></a>

If there any questions about my code or the project let me know.
