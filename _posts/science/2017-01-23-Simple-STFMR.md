---
layout: pagewmath
subheadline: Theory
title:  "Simple Spin-Torque Ferromagnetic Resonance (STFMR)"
teaser: "In which the fundamentals of STFMR are discussed."
breadcrumb: true
tags:
    - theory
categories:
    - science
image:
    thumb: STFMR/STFMRsim.jpg
    title: STFMR/STFMRsim.jpg
---

[Note: This page uses MathJax to render TeX equations]

# Introduction

Spin-Torque Ferromagnetic Resonance (STFMR or sometimes ST-FMR) is a divine and wonderful technique for extracting the strength of in-plane and out-of-plane torques exerted from some layer onto a magnetic layer. It's utility and broad use comes largely from the fact that it is a resonance phenomenon: the resulting signal is Lorentzian in nature. This is great because it means the signal is relatively easy to spot; and, even though the set of all possible Lorentzians (and their derivatives) do not form a complete, orthogonal basis, they are still pretty constrained from a fitting perspective.

# The Setup

In "classical" STFMR, we are looking at the motion of thin (~1-20 nm) film of a magnetic material with low coercivity, minimal anisotropy, and a large anisotropic magnetoresistance. The first two conditions simplify the experiment and the analysis by making valid the assumption that the equilibrium direction of the magnetization will lie in-plane and parallel to the applied external field. The last condition directly corresponds to how large the signal we detect will be as we will see later. 

This thin film of magnet is "grown" (a more physically correct term might be "deposited") either above or below a thin film of the material from which we are trying to extract information about its spin-torque generating properties. This stack of magnet+material-of-interest is then turned into a device which consists of a strip of the stack typically 5-10 $\mu$m wide and long with large metal contact pads at either end to facilitate interaction with the macroscopic experimental set up. The actual device length and width are chosen to minimize in-plane demagnetization fields (<em>i.e.</em> having a low-ish aspect ratio) and to have resistance as close to but greater than 50 Ohms as possible (to maximize RF power transmission). To do the experiment, we apply a radio frequency current at a fixed frequency (1-40 GHz), sweep an external magnetic field, and measure the DC voltage response.

For our analysis we will want to make a sensible choice of coordinates to simplify our lives. From a device perspective, it is natural to put $\hat{z}$ out of the plane and $\hat{y}$ along the direction of current flow through the device. While this choice seems somewhat natural, a more convenient choice is to put $\hat{y}$ parallel to the magnetization direction, keeping $\hat{z}$ the same (due to the demagnetization field of the thin film keeping the equilibrium magnetization direction in-plane). To keep track of this distinction, the natural device-based coordinates will be primed (<em>i.e.</em> $\hat{y}'$). Figure 1 lays this out graphically. Why make this choice? We expect the magnetization to oscillate around it's equilibrium direction so making its equilibrium direction correspond to an axis simplifies the number of directions we have to consider. To get back to the device coordinates, we'll just need to do a simple rotation matrix transformation (mostly, there are a few extra subtleties).

<figure>
<img src="{{ site.urlimg }}STFMR/coordinates.png">
<figcaption>Figure 1. Coordinate choice for our analysis. The primed coordinates are the actual device coordinates with $\hat{y}'$ parallel to the current flow direction. The unprimed $\hat{y}$ is defined to be parallel to the equilibrium magnetization direction.</figcaption>
</figure>



# The Math

## Landau-Lifschift-Gilbert-Slonczewski

Standing on the shoulders of giants, we find that the equations of motion which govern the motion of a magnet's magnetization are summarized by:

$$\dot{\overrightarrow{m}}=\alpha \overrightarrow{m}\times\dot{\overrightarrow{m}}+\overrightarrow{\tau_{eq}}+\overrightarrow{\tau_{neq}}$$

where $\overrightarrow{m}$ is $\overrightarrow{M}/M_s$( where $M_s$ is the saturation magnetization), $\alpha$ is the phenomenological Gilbert damping parameter, $\tau_{eq}$ are the equilibrium torques, $\tau_{neq}$ are the non-equilibrium torques, and dots denote time derivatives. Because of our sensible choice of coordinates, and the fact that we expect the magnet to deviate only slightly from its equilibrium position, we expect 

$$\overrightarrow{m}=(m_x,1,m_z)$$

and so 

$$\dot{\overrightarrow{m}}=(\dot{m}_x,0,\dot{m}_z)$$

Maybe this bothers you, the vector isn't really "unit" anymore if this is what we're going with. If it does, then this whole endeavor is going to be rough because that is just the first of many approximations we are going to be making. My response is to not worry, the torques on the magnet are sufficiently small that other assumptions tend to break down before the magnet is driven so far that the above breaks down.

## $\tau_{eq}$
<br>
The equilibrium torques are just a fancy name for what you probably expected (that is, what you might expect if you spend your days studying magnetism): the magnetization's dipole interaction with the external applied field and the out-of-plane demagnetization field. The first is just $$\gamma\hat{m}\times \overrightarrow{B}_{ext}$$ (where, by our coordinate definition in light of our assumptions, $$\hat{B}_{ext}\equiv\hat{y}$$); the second is $$\gamma M_{eff}\hat{m}\cdot\overline{N}_d$$, where $$\overline{N}_d$$ is the demagnetization tensor and $$M_{eff}$$ is the saturation magnetization plus any out-of-plane anisotropy. For a thin film such as ours, $\overline{N}_d$ has only one non-zero component, $$\overline{N}_{33}$$ (or $$\overline{N}_{zz}$$, more explicitly) = 1. Thus we have $$\gamma M_sm_z\hat{x}$$ as our demagnetization equilibrium torque. If we have any magnetocrystalline or other anisotropy, it would enter in here too. Luckily, prudent experimental choices about magnet type and field range mean such complications are typically irrelevant. What happens when our luck runs out is covered in a later post.

## $\tau_{neq}$
<br>
These torques are the torques that will drive the system into resonance. In fact, if you look at the LLGS equations from a more abstract perspective, what we have is a damped (literally the Gilbert damping), driven (these torques) harmonic oscillator. The second time derivative may not be immediately apparent because we have a system of equations owing to the vector nature of the problem, but solving for each component would make this manifest. This is why we will end up with Lorentzian lineshapes, they are the natural solutions to a damped, driven, harmonic oscillator system. 

In our system, the most general, linearly-independent torques we could imagine are a torque which acts out-of-plane (that is, acts like an in-plane field), $\tau_{FL}$; and a torque which acts in-plane (acts like an out-of-plane field), $\tau_{DL}$. The "FL" stands for "field-like," referring to how this torque points in the sample direction as the torque due to the external applied field. The "DL" stands for damping-like to highlight how special the in-plane torque is, it acts in the same direction as the damping term, which has deep implications for technological uses. In the simplest case, $\tau_{FL}$ arises solely from the fact that a current gives rise to a magnetic field. For our thin film, the current is, to a very good approximation, and infinite sheet of current and so gives rise to a uniform in-plane "Oersted" magnetic field of 

$$
B_{Oe}= \frac{\mu_0 I_{RF}}{2w}
$$

where $w$ is the width of the device, and $I_{RF}$ is the total RF current flowing through device. Of course, while this is literally correct and the resulting torque is $\gamma\overrightarrow{m}\times B_{Oe}$, it is not particularly instructive. Rather, we would like a source-agnostic definition that links current to torque. In that case, we might write

$$
\tau_{FL}=\frac{\gamma\hbar}{2 e M_s t} J_s
$$

where $\gamma$ is the magnet's gyromagnetic ratio (typically close to the electron value of about 28 GHz/T), $\hbar$ is Planck's constant, $e$ is the electron charge, $t$ is the thickness of the magnetic layer; and $J_s\equiv\theta_{FL}J_c$ is the spin current produced by the normal charge current $J_c$ with some conversion efficiency $\theta_{FL}$. 

This notational shift is useful because it allows us to reduce everything to a dimensionless figure of merit, $\theta_{FL}$, and because if allows us to account for other torques that are not due to the Oersted field but may act in the same direction. We may makes exactly the same definition of the magnitude of the damping-like torques by just replacing $\theta_{FL}$ with a different conversion efficiency, $\theta_{DL}$. Unfortunately, carrying around all of the prefactors is tedious and un-enlightening in the following derivation, so we'll actually just stick to $\tau_{FL}\hat{z}$ and $\tau_{DL}\hat{x}$.

## Into the Breach


It is most useful to consider the equations by direction, omitting the $\hat{y}$ piece, because it is zero.

$$
\dot{m}_x=\alpha m_y\dot{m}_z+\gamma(B_{ext}+M_{eff})m_z+\tau_{DL}
$$

$$
\dot{m}_z=-\alpha m_y\dot{m}_x-\gamma B_{ext}m_x+\tau_{FL}
$$

where I have already dropped the terms which would have $\dot{m}_y$ (as that is zero). Next, we may as well finish the invocation of our assumptions and replace the $m_y$ with 1.

$$
\dot{m}_x=\alpha \dot{m}_z+\gamma (B_{ext}+M_{eff})m_z+\tau_{DL}
$$

$$
\dot{m}_z=-\alpha\dot{m}_x-\gamma B_{ext}m_x+\tau_{FL}
$$

Now we need some help. We expect that this system will oscillate with some frequency so we may as well look for solutions of the type $$\|m_0\|e^{-i\omega t}$$. Doing this helps us with the time derivatives as they just pull down a factor of $-i\omega$ and leave behind an unchanged exponential, allowing us to cancel the factor of the exponential from every term. If we had had something weird going on where a term had two factors of $e^{-i\omega t}$ that would indicate a term that as oscillating at twice the frequency and not all. For reasons discussed in the next section, we only care about the terms with one factor of $e^{-i\omega t}$, so we would just discard all of the higher order "homodyne" terms. Doing this yields

$$
-i\omega m_x=-i\omega\alpha m_z+\gamma (B_{ext}+M_{eff})m_z+\tau_{DL}
$$

$$
-i\omega m_z=i\omega\alpha m_x-\gamma B_{ext}m_x+\tau_{FL}
$$

Now begins the slog. We need to solve for either $m_x$ or $m_z$. In light of setting up the punchline, we'll do $m_x$, but it of course doesn't matter. With one known, the other is also known through the above relations. Inserting $m_z$ into $m_x$ gives

$$
-i\omega m_x=\alpha(i\omega\alpha m_x-\gamma B_{ext}m_x+\tau_{FL})+(B_{ext}+M_{eff})\frac{\gamma}{-i\omega}(i\omega\alpha m_x-\gamma B_{ext}m_x+\tau_{FL})+\tau_{DL}
$$

rearranging,

$$
m_x(-i\omega-i\alpha^2\omega+\alpha\gamma B_{ext}+\alpha\gamma (B_{ext}+M_{eff})-\frac{\gamma^2}{i\omega}B_{ext}(B_{ext}+M_{eff}))=\alpha\tau_{FL}+(B_{ext}+M_{eff})\frac{\gamma}{-i\omega}\tau_{FL}+\tau_{DL}
$$

multiplying through by $i\omega$,

$$
m_x(\omega^2(1+\alpha^2)+i\omega\alpha\gamma(2B_{ext}+M_{eff})-\gamma^2B_{ext}(B_{ext}+M_{eff}))=i\omega\alpha\tau_{FL}-(B_{ext}+M_{eff})\gamma\tau_{FL}+i\omega\tau_{DL}
$$

$\alpha$ is typically around $10^{-2}$ or lower in the magnets we use so to an excellent approximation $1+\alpha^2\approx1$. Finally isolating $m_x$, we have

$$
m_x=\frac{i\omega\alpha\tau_{FL}-(B_{ext}+M_{eff})\gamma\tau_{FL}+i\omega\tau_{DL}}{\omega^2-\gamma^2B_{ext}(B_{ext}+M_{eff})+i\omega\alpha\gamma(2B_{ext}+M_{eff})}
$$

Let's make a convenient and prescient definition

$$
\omega_0\equiv\gamma\sqrt{B_{ext}(B_{eff}+M_{eff})}
$$

so that

$$
m_x=\frac{i\omega\alpha\tau_{FL}-(B_{ext}+M_{eff})\gamma\tau_{FL}+i\omega\tau_{DL}}{\omega^2-\omega_0^2+i\omega\alpha\gamma(2B_{ext}+M_{eff})}
$$

We're getting there, already we see a basically Lorentzian lineshape starting to form. To simplify things, lets multiply both the numerator and denominator by the complex conjugate of the denominator and separate the real and imaginary parts

$$
\Re[m_x]=\frac{\omega^2\alpha^2\gamma\tau_{FL}(2B_{ext}+M_{eff})-(B_{ext}+M_{eff})\gamma\tau_{FL}(\omega^2-\omega_0^2)+\omega^2\tau_{DL}\alpha\gamma(2B_{ext}+M_{eff})}{(\omega^2-\omega_0^2)^2+\omega^2\alpha^2\gamma^2(2B_{ext}+M_{eff})^2}
$$

$$
\Im[m_x]=\frac{\omega\alpha\tau_{FL}(\omega^2-\omega_0^2)+(B_{ext}+M_{eff})\gamma^2\omega\alpha\tau_{FL}(2B_{ext}+M_{eff})+\omega\tau_{DL}(\omega^2-\omega_0^2)}{(\omega^2-\omega_0^2)^2+\omega^2\alpha^2\gamma^2(2B_{ext}+M_{eff})^2}
$$

It turns out the real (in-phase with the driving current) part is actually what we care about and again, we drop the term with $\alpha^2$ to leave us with:

$$
\Re[m_x]=\frac{-(B_{ext}+M_{eff})\gamma\tau_{FL}(\omega^2-\omega_0^2)+\omega^2\tau_{DL}\alpha\gamma(2B_{ext}+M_{eff})}{(\omega^2-\omega_0^2)^2+\omega^2\alpha^2\gamma^2(2B_{ext}+M_{eff})^2}
$$

So what does this mean? We seem to have two terms in the numerator, one which doesn't change sign with field (the $$\tau_{DL}$$ term) and one which does (the $$\tau_{FL}$$ term). This gives rise to two linearly independent contributions to $m_x$ on which is symmetric through resonance (no sign change) and one which is antisymmetric through resonance (sign change). This is what we came for. Each type of non-equilibrium torque gives rise to a distinct line shape so we can theoretically get information on them both by fitting the resulting signal to the sum of the two types of Lorentzians.

# Back to Reality

All of this has been great, but there is still a problem, this natural  frequency $\omega_0$ is on the order of GHz. That's a tough thing to measure, so can we even utilize the beautry of the equations to do science? Yes. Luckily, we can exploit a neat property of most magnets, anisotropic magnetoresistance or AMR. In the case of the most commonly used STFMR magnet, Permalloy (Py, Ni$$_{80}$$Fe$$_{20}$$), the resistance of the magnet is higher when the magnetization is parallel to the current flow, and lowest when perpendicular to the current flow. Since we are driving the magnetization into oscillation, that means that the resistance of the device is change at the same frequency. But we are also applying a RF current at the same frequency to drive the system! That means we must get a voltage out at twice the frequency and zero frequency. Bam. We apply an RF current and measure a DC voltage, a beautiful turn of experimental events. This is also why choosing a magnetic material with a large AMR is beneficial.

Ultimately we get an equation like

$$
V_{\text{mix}}=(-2\cos^2\phi\sin\phi)\frac{1}{2}\frac{dR}{d\phi}\frac{-(B_{ext}+M_{eff})\gamma\tau_{FL}(\omega^2-\omega_0^2)+\omega^2\tau_{DL}\alpha\gamma(2B_{ext}+M_{eff})}{(\omega^2-\omega_0^2)^2+\omega^2\alpha^2\gamma^2(2B_{ext}+M_{eff})^2}
$$

where the $\frac{1}{2}\frac{dR}{d\phi}$ comes from the $\cos^2(\omega t)$ that mixes down to a DC component with $\frac{dR}{d\phi}$ being the amplitude of the $\cos^2\phi$ dependence of the AMR, one factor of $\cos\phi$ comes from the cross product of magnetization with the effective and real fields that give rise to the torques, and the remaining $-2\cos\phi\sin\phi$ comes from the derivative of the $\cos^2\phi$ AMR. Putting everything together and doing an experiment yields something like Figure 2 ([from a recent paper I coauthored][1]), which shows the total fit to the data and the separate contributions of each Lorentzian separately.

<figure>
<img src="{{ site.urlimg }}STFMR/STFMR.jpg">
<figcaption>Figure 2. Acual STFMR data from a device with a permalloy magnetic layer and the associated fit from a recent paper I coauthored. </figcaption>
</figure>


## Related projects
{: .t60 }
{% include list-posts tag='theory' %}

[1]: https://arxiv.org/abs/1612.01927

