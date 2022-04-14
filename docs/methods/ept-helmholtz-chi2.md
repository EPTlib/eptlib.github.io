---
layout: default
title: Phase-based Helmholtz EPT with automatically selected kernel
nav_order: 20
parent: Methods
usemathjax: true
description:
permalink: /methods/ept-helmholtz-chi2
last_modified_date: 2022-04-14T13:54:08+0200
---

# Phase-based Helmholtz EPT with automatically selected kernel
{: .no_toc .fs-9 }

Experimental
{: .label .label-yellow .mt-3}

## Table of contents
{: .no_toc .text-delta }

1. TOC
{: toc}

---

## Theory

The implemented method is based on the phase-based approximation of [Helmholtz EPT](/methods/ept-helmholtz), that retrieves the electric conductivity from the transceive phase input according to the relation
\$$
\sigma = \frac{\nabla^2 \varphi^{\pm} }{2 \omega \mu_0}\,.
\$$
In this case, besides a constant multiplicative coefficient, the estimation of the electric conductivity $$\sigma$$ consists in the evaluation of the Laplacian of the transceive phase $$\varphi^\pm$$.

The Laplacian of the noisy acquisition of $$\varphi^\pm$$ is estimated numerically with a Savitzky-Golay filter [[1](#references)]. It approximates the phase map around the voxel of interest with a paraboloid (or a higher order polynomial), of which computes the derivatives. In this way, the Savitzky-Golay filter reduces the impact of noise in the Laplacian computation.

The polynomial fitting on which the Savitzky-Golay filter is based depends on the definition of a kernel of voxels around the voxel of interest. The kernel should be large enough to reduce the noise propagation, but not too large to include voxels from adjacent tissues. In the latter case, the assumption of local homogeneity on which Helmholtz EPT relies would not hold and, therefore, large systematic errors would appear.
This implementation of the phase-based Helmholtz EPT looks automatically for the optimal kernel given a set of options.

In each voxel, the kernel is selected assuring that the estimated conductivity is physically admissible (i.e., non-negative) and that the uncertainty associated with the estimated conductivity is minimum. The uncertainty is evaluated by propagating it through the fitting under the assumption of independent and identically distributed noise in each voxel of the acquired phase map.

### References
{: .no_toc}

[1] A. Savitzky and M.J.E. Golay, "Smoothing and differentiation of data by simplified least squares procedures," _Analytical Chemistry_, 36(8):1627-39, 1964. DOI: [10.1021/ac60214a047](https://doi.org/10.1021/ac60214a047)
{: .fs-3}

## Settings

Being a phase-based method, the [input address](/settings#input) ```trx-phase``` must be set.

Moreover, ```tx-channel``` and ```rx-channel``` must be equal to 1.

The following specific settings must be configured.

### Savitzky-Golay filter

A list of kernels (characterised by the size and the shape) on which the Savitzky-Golay filter can be applied must be provided.

```toml
[parameter.savitzky-golay]
    sizes = [[2, 2, 1], [3, 3, 1], [3, 3, 1]]
    shapes = [1, 1, 2]
    degree = 2
	output-index = "example.h5:/index"
```

- ```sizes``` is the list of the sizes of the kernels. The size defines the number of voxels along each semi-axis of the kernel. This setting is mandatory.
- ```shapes``` is the list of the shapes of the kernels. The shape is defined according to the following table. This setting is mandatory.
- ```degree``` is the degree of the interpolating polynomial. It is the same for all the kernels. It must be at least 2 (default: ```2```).
- ```output-index``` is the address where the map of the selected kernels will be written. It must be a dataset in an .h5 file. The map contains in each voxel the index of the selected kernel ordered as in the ```sizes``` and ```shapes``` lists starting from the index 0. If omitted, the map will not be stored.

| Code | Shape     | Example with ```size = [2,2,1]```                                        |
|------+-----------+--------------------------------------------------------------------------|
| 0    | Cross     | ![](/assets/images/savitzky-golay-cross.png){: style="width:128px"}      |
| 1    | Ellipsoid | ![](/assets/images/savitzky-golay-ellipsoid.png){: style="width:128px"}  |
| 2    | Cuboid    | ![](/assets/images/savitzky-golay-cuboid.png){: style="width:159px"}     |

### Other parameters

```toml
[parameter]
    output-variance = "example.h5:/var"
```

- ```output-variance``` is the address where the variance evaluated for the electric conductivity will be written. It must be a dataset in an .h5 file. If omitted, the variance will not be stored.
