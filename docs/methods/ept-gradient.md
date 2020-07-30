---
layout: default
title: Gradient EPT
nav_order: 15
parent: Methods
usemathjax: true
description:
permalink: /methods/ept-gradient
last_modified_date: 2020-07-30T10:50:54+02:00
---

# Gradient EPT
{: .no_toc .fs-9 }
Under development
{: .label .label-yellow }

EPT method for parallel transmission imaging systems. It requires that the input is acquired near the mid-plane of a longitudinally symmetric transmit coil.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{: toc}

---

## Theory

A brief introduction to gradient EPT is provided below. More details can be found in [[1](#references)].

### First step (local estimation)

Gradient EPT is built on top of the same equation of [convection-reaction EPT](/methods/ept-convreact#theory) repeated for each transmit channel of the parallel transmission system.
Thus, it relies on the same hypothesis of convection-reaction EPT: the gradient of $$\nabla H_{i,z} \simeq 0$$ is negligible for any transmit channel $$i$$.
This assumption is verified any time the RF coil is longitudinally symmetric, which is very often the case for volume coils.

For convenience, the convection-reaction EPT equation is rewritten, for the $$i$$-th transmit channel, as
\$$
\omega^2 \mu_0 B_{1,i}^+ \tilde{\varepsilon} + \nabla^2 B_{1,i}^+ = \nabla B_{1,i}^+ \cdot \left( g_+, -{\rm i} g_+, g_z \right)\,,
\$$
where $$g_+ = g_x + {\rm i} g_y$$ and $${\bf g} = \nabla \log(\tilde{\varepsilon})$$.

The problem with parallel transmission is that, in general, it is impossible to estimate the complex transmit sensitivity $$B_{1,i}^+$$, because there is no way deduce the phase of the $$i$$-th transmit sensitivity $$\varphi^+_i$$ from the measured transceive phase $$\varphi^\pm_i = \varphi^+_i + \varphi^-$$, which entangles $$\varphi^+_i$$ with the phase of the receive sensitivity $$\varphi^-$$.

However, being the receive sensitivity the same in all the acquired transceive phases, it is possible to estimate the relative phases as
\$$
\varphi_i^{+,{\rm r}} = \varphi_i^\pm - \varphi_0^\pm = \varphi_i^+ - \varphi_0^+\,,
\$$
where $$\varphi_0^+$$ has been chosen as the reference phase.
Thus, the $$i$$-th complex transmit sensitivity can be written as
\$$
B_{1,i}^+ = B_{1,i}^{+,{\rm r} } {\rm e}^{ {\rm i} \varphi_0^+ }\,,
\$$
where $$B_{1,i}^{+,{\rm r} } = \vert B_{1,i}^+ \vert {\rm e}^{ {\rm i}\varphi_i^{+,{\rm r} } }$$ is the measurable part.

Thus, the starting equation can be elaborated by splitting the measurable and the unknown parts of the complex transmit sensitivity to obtain
\$$
\boxed{ -{\rm i} 2 \nabla B_{1,i}^{+,{\rm r} } \cdot \nabla \varphi_0^+ + \nabla B_{1,i}^{+,{\rm r} } \cdot \left( g_+, -{\rm r} g_+, g_z \right) + B_{1,i}^{+,{\rm r} } \vartheta^+ = \nabla^2 B_{1,i}^{+,{\rm r} } }\,,
\$$
where $$\nabla \varphi_0^+$$, $$g_+$$, $$g_z$$ and $$\vartheta^+$$ are treated as algebraically independent unknowns. In particular, $$\vartheta^+ = -\omega^2 \mu_0 \tilde{\varepsilon} + \vert \nabla \varphi_0^+ \vert^2 - {\rm i} \nabla^2 \varphi_0^+ + {\rm i} \nabla \varphi_0^+ \cdot \left( g_+, -{\rm i} g_+, g_z \right)$$ collects all the non-linear unknown terms and its combination with the other unknown allows retrieving a first estimate of the electric properties $$\tilde{\varepsilon}$$.

The latter equation is solved in a voxel-by-voxel fashion by combining its evaluations for each transmit channel in a linear system.
The linear system is solved as many times as the number of transmit channels, every time using a different channel for the reference phase. The results, weighted by the magnitude of the transmit sensitivity of the reference channel, are then averaged to reduce the effect of numerical errors, as described in [[2](#references)].

### Second step (global estimation)

The solution of the previous system of equations provides in each voxel a first local estimate of the electric properties $$\tilde{\varepsilon}_0$$ as well as an estimate of the derivatives of its logarithm $$g_{+,0}$$ and $$g_{z,0}$$.

The retrieved distributions of the derivatives can be elaborated to provide a global estimate of the electric properties.
In the EPTlib application, the global estimate is obtained by solving the minimisation problem
\$$
    \boxed{ \tilde{\varepsilon} = {\rm argmin} \left( \left\vert g_+(\tilde{\varepsilon}) - g_{+,0} \right\vert^2 + \left\vert g_z(\tilde{\varepsilon}) - g_{z,0} \right\vert^2 \right) }\,,
\$$
with the constraint of a given value of $$\tilde{\varepsilon}$$ in a selected voxel.
The couple of the voxel and the associated constraint value of $$\tilde{\varepsilon}$$ is referred to as a _seed point_.

The minimisation is performed by solving the associated linear Euler equation with the finite element method (FEM).

Other techniques, that avoid the use of seed points by relying on the estimate $$\tilde{\varepsilon}_0$$ or on the result of other EPT methods, could be adopted, like the ones described in [[3](references)] or in [[4](references)], but they are not implemented in EPTlib, yet.

### Two-dimensional approximation

In order to reduce the computational cost of the method, a two-dimensional approximated equation can be obtained by assuming that the properties are almost constant along the longitudinal direction (i.e., $$g_z \simeq 0$$) as well as the reference phase (i.e., $$\partial_z \varphi_0^+ \simeq 0$$). In this case, the equation for the local estimate is
\$$
    \boxed{ -{\rm i} 2 \nabla_{xy} B_{1,i}^{+,{\rm r} } \cdot \nabla_{xy} \varphi_0^+ + \nabla_{xy} B_{1,i}^{+,{\rm r} } \cdot \left( g_+, -{\rm r} g_+ \right) + B_{1,i}^{+,{\rm r} } \vartheta_{xy}^+ = \nabla^2 B_{1,i}^{+,{\rm r} } }\,,
\$$
with $$\vartheta_{xy}^+ = -\omega^2\mu_0 \tilde{\varepsilon} + \|\nabla_{xy} \varphi_0^+ \|^2 - {\rm i} \nabla_{xy}^2 \varphi_0^+ + {\rm i} \nabla_{xy} \varphi_0^+ \cdot (g_+, -{\rm i}g_+)$$.

In this case, also the global estimate is performed in two dimensions by solving the minimisation problem
\$$
    \boxed{ \tilde{\varepsilon} = {\rm argmin}\ \left\vert g_+(\tilde{\varepsilon}) - g_{+,0} \right\vert^2 }\,,
\$$
with the constraint provided by a seed point. In the latter equation, $$g_{+,0}$$ denotes the estimate of the derivative obtained by solving the previous linear system pixel-by-pixel.

### Derivatives estimation

Provided a noisy measurement of the distribution of $$B_1^+$$, or $$\varphi^{\pm}$$, its derivatives can be estimated with a second-order Savitzky-Golay filter [[5](#references)]. It approximates the distribution around the voxel of interest with a paraboloid, of which computes the derivatives. In this way, the Savitzky-Golay filter reduces the impact of noise in the derivatives computation.

### References
{: .no_toc}

[1] A. Arduino, "Mathematical methods for magnetic resonance based electric properties tomography," PhD thesis, Politecnico di Torino, 2018. DOI: [10.6092/polito/porto/2698325](https://doi.org/10.6092/polito/porto/2698325)
{: .fs-3 }

[2] J. Liu, X. Zhang, S. Schmitter, P.-F. Van de Moortele and B. He, "Gradient-based electrical properties tomography (gEPT): a robust method for mapping electrical properties of biological tissues in vivo using magnetic resonance imaging," _Magnetic Resonance in Medicine_, 74(3):634-46, 2015. DOI: [10.1002/mrm.25434](https://doi.org/10.1002/mrm.25434)
{: .fs-3}

[3] J. Liu, Q. Shao, Y. Wang, G. Adriany, J. Bischof, P.-F. Van de Moortele and B. He, "In vivo imaging of electrical properties of an animal tumor model with an 8-channel transceiver array at 7 T using electrical properties tomography," _Magnetic Resonance in Medicine_, 78(6):2157-69, 2017. DOI: [10.1002/mrm.26609](https://doi.org/10.1002/mrm.26609)
{: .fs-3}

[4] Y. Wang, P.-F. Van de Moortele and B. He, "CONtrast Conformed Electrical Properties Tomography (CONCEPT) based on multi-channel transmission and alternating direction method of multipliers," _IEEE Transactions on Medical Imaging_, 38(2):349-59, 2019. DOI: [10.1109/TMI.2018.2865121](https://doi.org/10.1109/TMI.2018.2865121)
{: .fs-3}

[5] A. Savitzky and M.J.E. Golay, "Smoothing and differentiation of data by simplified least squares procedures," _Analytical Chemistry_, 36(8):1627-39, 1964. DOI: [10.1021/ac60214a047](https://doi.org/10.1021/ac60214a047)
{: .fs-3}

## Settings

Both the [input addresses](/settings#input) are mandatory.

```tx-channel``` must be at least equal to 5.

```rx-channel``` must be equal to 1.

The following specific settings must be configured.

{% include_relative savitzky-golay.md %}

### Seed point

Only one seed point can be set. The seed point is used only if the full run of gradient EPT is executed.

```toml
[parameter.seed-point]
    coordinates = [15,20,25]
    electric-conductivity = 0.1 # [S/m]
    relative-permittivity = 50.0
```

- ```coordinates``` is the list of the integer coordinates of the voxel where the seed point is set (default: ```[0,0,0]```).
- ```electric-conductivity``` is the value of the electric conductivity forced in the seed point in siemens per meter (default: ```0.0```).
- ```relative-permittivity``` is the value of the relative permittivity forced in the seed point (default: ```1.0```).

### Other parameters

```toml
[parameter]
    volume-tomography = false
    imaging-slice = 3
    full-run = true
```

- ```volume-tomography``` is equal to ```true``` to solve the three-dimensional problem, otherwise, for the two-dimensional approximation, it is equal to ```false``` (default: ```false```).
- ```imaging-slice``` is the index of the slice on which perform the two-dimensional tomography. Must be set only if ```volume-tomography``` is ```false``` (default: the index of the mid-plane).
- ```full-run``` is equal to ```true``` to execute the full run of gradient EPT; otherwise, it is equal to ```false``` to stop the execution after the first local estimate of the electric properties (default: ```true```).
This feature is useful in experimenting with the method implementation. It will probably be removed in a future release of the software.

## Example

The following configuration file is used to perform the gradient EPT of a heterogeneous phantom. The input transmit sensitivities and transceive phases have been obtained by simulating the phantom in a volume coil designed for parallel transmission at 297 MHz, that is the Larmor frequency of a 7 T scanner. The same coil has been used in reception with a circular polarization.

The configuration file can be downloaded [here](/assets/examples/ept-gradient.toml).
The input .h5 file containing the data can be downloaded [here](/assets/examples/heterogeneous-phantom-7t-ptx.h5).

```toml
{% include_relative ept-gradient.toml %}
```

The retrieved distribution of the electric conductivity is pictured in the following image, together with the expected distribution and the input maps.
In order to determine the seed point, it has been assumed that the properties of the phantom background were known.

![](/assets/images/ept-grad.png)
