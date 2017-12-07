---
layout: page
subheadline: Methods
title:  "Building a digitially-controlled, optically-isolated current source"
teaser: "In which ground loops are sleuthed and switching power supplies are replaced"
breadcrumb: true
tags:
    - Circuits
    - Systems
    - ST-FMR
categories:
    - science
    - projects
image:
#    thumb: EDCsash/allon.jpg
#    title: EDCsash/allon-square.jpg
---

One of the techniques that I used semi-frequently in my research is DC-biased Spin Torque Ferromagnetic Resonance (ST-FMR). While a discussion of the technique itself merits a separate post, suffice it to say that one of the chief difficulties associated with this technique is the need for an additional source of DC to add to the GHz AC already present in ST-FMR. 

There are several solutions to this problem. One is to use a potentiometer (technically a rheostat in the simplest case), battery, and ammeter in series with your sample to dial in a current and then do a magnetic field sweep to get data. This route is fast, simple, low-noise, and reallllllllly tedious. In particular, one typically wants to do many scans at difference DC-biases, which means such a two-dimensional scan (stepping current and sweeping field for the ST-FMR part of DC-biased ST-FMR) requires the dedicated experimentalist paying reasonably close attention and staying nearby the experiment for what may be upwards of several hours.

In the scheme of experiences when it comes to doing science, there are certainly worse experiences one could have. Given that it's the 21st century though, one might wonder if we can automate the current sweeping. It turns out for the well-stocked research lab, there are several options for doing so. The simplest in our case was to use a Keithley 2400 source meter in current sourcing mode. This solution does do what's advertised: digital control of the current source is straightfoward and well-documented. What this solution also does is add a copious amount of noise to the experiment. Now this not necessarily a reflection on Keithley, they make quality products in my experience. Rather, what it is is a statement of poorly organized grounds. In particular, the RF signal generator ground can now talk to the Keithley ground which ends up making a ground loop. Furthermore, it is a switching power supply type device so the high frequency components of the square wave pulse width modulation maybe also contribute a small amount.

So, these two experiences led me to what I hope is the natural question: Why not both?

## Doing Both

Ultimately, the goal of this endeavor is to build a current source circuit which is digitally controlled, but whose ground for the serial transmission to a computer is not the same as the ground of the current source. What this means is that everything on the control side will need to be optically or capacitively isolated from everything on the current source side. For much greater cost, a fiber-optic usb cable could be used or a USB isolator, but it's both cheaper and more instructive to do it the way first mentioned. 

### Sourcing Current

To source the current I considered several options. For controlling high-power loads, a common option is the op-amp to base of transistor with load and current sensing feed back resistor in series. The current is then set by the op amp trying to maintain a voltage drop across the sensing resistor equal to the voltage supplied to it's non-inverting input. This is cool and all, by there is a finite voltage drop across the transistor and there will always be a small amount of current flowing into the sample from the base which affects precision. Luckily, I am only trying to control up to 20 mA at most so I can use the ouput of the op amp as the current source itself. 

### A New Op-Amp

With this decision made, I faced another problem I hadn't yet encountered in my physics career: how to get around needing a negative voltage. In order for the standard and venerable op-amps of yore lying around my lab to have linear closed loop gain through 0 V, a negative voltage is required. I had resolved to only power my current source with a 12 V battery and I didn't want to give up any of that potential to create a virtual ground with a few volts negative beneath. Lo, it turns out in the decades since my lab acquired its supply of op-amps, this problem has been routinely encountered and solved with the magic of "rail-to-rail" op-amps. These marvels of electrical engineering are linear to their rails (0 V and +Vcc). Phew. Problem solved. 

## Current Control

The controller of the current source will be, again, non-other than the venerable arduino microcontroller. In order to control the current using the op-amp I had to find a way to generate a smooth, DC voltage to supply to the op-amp to set the current as described above. At first, I tried to use a digitally controlled potentiometer as the voltage source with the wiper voltage with respect to ground setting the current. I was able to get the potentiometer to behave as expected by itself on a prototyping board, but I couldn't get it to work as expected as part of the isolated circuit.

Then I discovered these motorized linear potentiometers[1]. Oh my. These are pretty neat and they solve the problem of robustly setting the current by being able to physically change the value a potentiometer. 

To make the system closed loop, I added in a 10 bit ADC that reads out the voltage across the sensor resistor in the op-amp circuit. This gives me around 10 microamp precision, nominally, given my choice of resistors.






## Related projects
{: .t60 }
{% include list-posts tag='ST-FMR' %}

[1]: https://www.sparkfun.com/products/10976


