---
layout: page
subheadline: Methods
title:  "Recovering a Windows® 2000 installation with 7 serial ports"
teaser: "In which I virtualized the operating system of a critical control computer for our sputter deposition system."
breadcrumb: true
tags:
    - Systems
categories:
    - projects
image:
    thumb: sputter_recovery/1ingunCornellsquare.jpg
    title: sputter_recovery/1ingunCornellsquare.jpg
---

One the tools that I use frequently in my research is a 7 gun magnetron sputter deposition. It may be because I have spent many hours learning its secrets and fixing it, but I have really come to appreciate what a feat of engineering this system was when it was built in the late 90's. Unfortunately, because it was built in the late 90's, it has begun to show its age. Most critically, this age shows in the control system and control software. The user interface was designed in a program that came as part of the Word Perfect suite back in the day, Paradox 9. Futhermore, the operating system it's running on is Windows® 2000 and the computer running the OS still uses IDE hard drives. 

## Failure Modes

A not uncommon failure mode of the system is a hard drive failure. This is fairly straightforward to recover from as we have a reliable back up on hand, any difficulty is largely in acquiring an IDE hard drive. That, however, is not insuperable. A new and more terrifying failure mode is the apparent failure of the motherboard hosting said hard drive. In their luminous foresight, the system engineers had included with the system a back up computer so this failure mode was reasonably straightforward to recover from. Alas, you can only have so many back ups, and age too had ravished the back up and it soon failed as well after re-installation. This failure of the backup was a non trivial problem from which to recover. 

Beginning with the failure of the primary computer, I had made some attempt to virtualize the Windows® 2000 OS and run the system on a new computer with a more recent OS. I had moderate success, but I ran into difficulty trying to get a full restore of the command computer system to boot in the virtual system using Oracle's [VirtualBox][1]. Evidently something in the boot records of the backup disagreed with VirtualBox. With a little more caution and selective recovery of was appeared to be files relevant to the control software using good ol' NTbackup, I was to successfully boot. However, attempts to open the Paradox 9 software left me with persistent "An attempt was made to access an unnamed file past its end" error messages.

## Paradox 9

<blockquote>
Nine Paradoxes:<br />
-A Cretan says: All Cretans are liars.<br />
-Is the word "heterological", meaning "not applicable to itself", a heterological word?<br />
-Moderation in all things, including moderation.<br />
-What would happen if Pinocchio said "My nose will be growing?"<br />
-A barber (who is a man) shaves all and only those men who do not shave themselves. Does he shave himself?<br />
-If you remove a single grain of sand from a heap, you still have a heap. Keep removing single grains, and the heap will disappear. Can a single grain of sand make the difference between heap and non-heap?<br />
-The next statement is true. The previous statement is false.<br />
-All I know is that I know nothing.<br />
-This statement is false. 
</blockquote>



Luckily, Paradox 9 and Paradox in general has a long and storied history, evidently the go-to software for databases designed in the 90's. For this reason, there is still an active community of Paradox developers and users. In particular, [the California Department of Health Care Services has a copy of the Paradox 9 runtime for download][2], presumably, because there is some legacy software in use. Whatever the reason, its visible presence on the internet was like a shimmering beacon in an oppressively dark night as I thought it was the Paradox installation that was bad. Unfortunately, that was not the problem; it seems this beacon was more of an <em>ignus fatuus</em>, in light of my previous simile.

Scouring further the internet, I came across [this post][5] on the Prestwood IT website which literally adresses that specific error. It turns out that the toolbar files were corrupted and the solution was to delete (or rename) all files of the type pdx_en*.cfg. Doing this stopped the error messages at the price of having toolbars on by default: an acceptable bargain. The next step was find a way to tunnel all seven of the serial ports expected by the software from the host OS into the virtual machine.

## Serial Port Redirection

As an aside, if you expressed incredulity at the number of serial ports, it is an incredulous fact. Even in the 90's computers only had 1-2 serial ports natively. In order to connect to all of the instruments required, a total of seven were required to communicate with all of the required systems. To achieve that many, a [Comtrol RocketPort EXPRESS][3] was used to break out a PCI slot to 16 serial ports. In another stroke of luck/good business practice, Comtrol still has available for downloads its Windows® 2000 drivers, though they are understandably no longer maintained. 

At this point there were two options for proceeding. Oracle's VirtualBox manual mentions that in some newer builds of Linux with some newer motherboards, the virtual operating system can use a card in a PCI slot without the host needing to have drivers for it. This would be perhaps an elegant and robust solution to the problem of getting the serial port data to the virtual machine, but it would require finding a suitable computer. As every moment the system was down delayed the advancement of the sum of knowledge, such a delay for the acquisition of such a system was untenable. The other option was to use a serial port virtualization software like eterlogic's [Virtual Serial Port Emulator (VSPE)][4]. This software allows for the passing of virtual serial ports through TCP/IP. The most straightfoward thing to do was then to create TCP servers connected to the appropriately named serial ports inside the virtual W2K OS and then use VirtualBox's Host-Only Network to talk to TCP clients set up by the same software running on the host OS connected to the RocketPort serial ports. The Host-Only Network has the advantage that it doesn't expose the virtual Windows® 2000 OS to the internet if we decide we need to connect the control computer to the internet.

Getting the VSPE TCP network linking to work through the host only network was remarkably easy. I just check the W2K ip address with <code>ipconfig</code> and followed the instructions in the VSPE documentation for setting up a network link, giving each virtual serial port on the virtual machine a different port. In an unsurprising turn of events, things did not work right away. I had the serial port settings wrong for four of the six instruments and had to look up their defaults on the internet. In one of the cases, the port config was not set to default parameters, but checking the settings on the instrument would have required taking the system apart (I'm looking at you Granville-Phillips 307 gauge monitor) so I iterated through sensible parities and bauds based on the options in the manual until it worked (I supposed I could have broken out an oscilloscope, but the set of possible combinations was fairly small). In the case of the seventh, most critical port, the one monitoring interlock signals, I had signal fidelity issues. 

## Serial Port Monitoring

The way I diagnosed this problem was through another piece of nice software: [Serial Port Monitor][7] by Eltima. Monitoring the traffic, it seems the command software directs a series of numbers as a query to the interlock computer and then the interlock computer responds with a set of numbers as a response. It seemed to me that this is a signal fidelity test as when the command software shows successful communication with the interlock computer the serial traffic is much more of what one might expect. Staring at the call and response during failed communication, the response showed corruption in at least one place most of the time suggesting that the USB to serial converter I was using was having no luck reliably interpreting the return signal from the interlock computer. To surmount this problem, I replaced the USB to RS232 converter with a [PCIe to RS232 card][8]. I also made use of VirtualBox's serial port feedthrough option to directly pass the interlock computer's serial port through to the virtual computer to decrease latency and to hopefully increase signal fidelity.

## Final Touches

With the 6 of the 7 serial ports bridged through TCP and VSPE and the interlock computer passed through virtualbox directly, I was able to reach the same state the system was in before it failed...almost. It seems that the VSPE can't handle 6 serial ports at a time when they are all being queried, the software freezes and fails during several deposition-related commands. To fix this I routed one more heavily polled serial port through VirtualBox's serial port pass-through function. So with five out of seven serial ports bridge over a host-only network, the sputter deposition system is now one half in the 21st century.

Post Title Image: The inside of the sputter chamber illuminated by the plasma of a 1" sputter gun.



## Related projects
{: .t60 }
{% include list-posts tag='Systems' %}

[1]: https://www.virtualbox.org/wiki/VirtualBox
[2]: http://www.dhcs.ca.gov/provgovpart/Pages/Runtime_9.aspx
[3]: http://www.comtrol.com/rocketport-multi-port-serial-cards/rocketport-universal-pci/rocketport-universal-pci-16port
[4]: http://www.eterlogic.com/help/vspe/NetworkBridgePage.html
[5]: http://www.prestwoodboards.com/aspsuite/eboard/thread.asp?MBID=9067
[6]: {{site.url}}/wildcard
[7]: http://www.eltima.com/products/serial-port-monitor/
[8]: https://www.amazon.com/dp/B00006B8C0/ref=psdc_3015425011_t2_B001VSR9TK


