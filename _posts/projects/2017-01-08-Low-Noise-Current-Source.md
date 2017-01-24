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
    - projects
image:
#    thumb: EDCsash/allon.jpg
#    title: EDCsash/allon-square.jpg
---

{% include alert warning='This post is not finished.' %}

One of the techniques that I used semi-frequently in my research is DC-biased Spin Torque Ferromagnetic Resonance (ST-FMR). While a discussion of the technique itself merits a separate post, suffice it to say that one of the chief difficulties associated with this technique is the need for an additional source of DC to add to the GHz AC already present in ST-FMR. 

There are several solutions to this problem. One is to use a potentiometer (technically a rheostat in the simplest case), battery, and ammeter in series with your sample to dial in a current and then do a magnetic field sweep to get data. This route is fast, simple, low-noise, and reallllllllly tedious. In particular, one typically wants to do many scans at difference DC-biases, which means such a two-dimensional scan (stepping current and sweeping field for the ST-FMR part of DC-biased ST-FMR) requires the dedicated experimentalist paying reasonably close attention and staying nearby the experiment for what may be upwards of several hours.

In the scheme of experiences when it comes to doing science, there are certainly worse experiences one could have. Given that it's the 21st century though, one might wonder if we can automate the current sweeping. It turns out for the well-stocked research lab, there are several options for doing so. The simplest in our case was to use a Keithley 2400 source meter in current sourcing mode. This solution does do what's advertised: digital control of the current source is straightfoward and well-documented. What this solution also does is add a copious amount of noise to the experiment. Now this not necessarily a reflection on Keithley, they make quality products in my experience. Rather, what it is is a statement of poorly organized grounds. In particular, the RF signal generator ground can now talk to the Keithley ground which ends up making a ground loop. Furthermore, it is a switching power supply type device so the high frequency components of the square wave pulse width modulation maybe also contribute a small amount.

So, these two experiences led me to what I hope is the natural question: Why not both?

## Doing Both

Ultimately, the goal of this endeavor is to build a current source circuit which is digitally controlled, but whose ground for the serial transmission to a computer is not the same as the ground of the current source. What this means is that everything on the control side will need to be optically or capacitively isolated from everything on the current source side. For much greater cost, a fiber-optic usb cable could be used or a USB isolator, but it's both cheaper and more instructive to do it the way first mentioned. 

### Sourcing Current

To source the current I considered several options. For controlling high-power loads, a common option is the venerable op-amp to base of transistor with load and current sensing feed back resistor in series. The current is then set by the op amp trying to maintain a voltage drop across the sensing resistor equal to the voltage supplied to it's non-inverting input. This is cool and all, by there is a finite voltage drop across the transistor and there will always be a small amount of current flowing into the sample from the base which affects precision. Luckily, I am only trying to control up to 20 mA at most so I can use the ouput of the op amp as the current source itself. 

### A New Op-Amp

With this decision made, I faced another problem I hadn't yet encountered in my physics career: how to get around needing a negative voltage. In order for the standard and venerable op-amps of yore lying around my lab to be linear through 0 V, a negative voltage is required. I had resolved to only power my current source with a 12 V battery and I didn't want to give up any of that potential to create a virtual ground with a few volts negative beneath. Lo, it turns out in the decades since my lab acquired its supply of op-amps, this problem has been routinely encountered and solved with the magic of "rail-to-rail" op-amps. These marvels of electrical engineering are linear to their rails (0 V and +Vcc). Phew. Problem solved. 


The controller of the current source will be, again, non-other than the venerable arduino microcontroller. The current will be sourced by a 'rail-to-rail' op-amp with feed back across a resistor to ground to set the current through the load in-series with said resistor. This leaves several options for both switching current direction and setting the current. One initial idea was essential a full-bridge rectifier and two copies of the op-amp current source oriented in opposite directions. This design proved untenable because the require number of diodes decreased the voltage available for applying current to too little given the 12 V battery powering everything. What was settled on was a home built MOSFET H-bridge, as it allows for only one copy of the op-amp circuit and there is very little voltage drop of the MOSFETS.





## Related projects
{: .t60 }
{% include list-posts tag='ST-FMR' %}

[1]: https://www.virtualbox.org/wiki/VirtualBox
[2]: http://www.dhcs.ca.gov/provgovpart/Pages/Runtime_9.aspx
[3]: http://www.comtrol.com/rocketport-multi-port-serial-cards/rocketport-universal-pci/rocketport-universal-pci-16port
[4]: http://www.eterlogic.com/help/vspe/NetworkBridgePage.html
[5]: http://www.prestwoodboards.com/aspsuite/eboard/thread.asp?MBID=9067
[6]: {{site.url}}/wildcard
[7]: http://www.eltima.com/products/serial-port-monitor/



