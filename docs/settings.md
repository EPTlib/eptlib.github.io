---
layout: default
title: Settings
nav_order: 2
description:
permalink: /settings
last_modified_date: 2022-04-14T13:40:03+0200
---

# Configuration file settings
{: .no_toc .fs-9}

The EPTlib application is configured by a .toml file.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{: toc}

---

In .toml files, the symbol ```#``` marks the rest of the line as a comment, except when inside a string.

## Title and annotations

```toml
title = "Example"
description = "Just an example of configuration file"
```

The ```title``` and the ```description``` will appear in the log of the EPTlib application.

## Method <object name="new" class="label">New!</object>

```toml
method = 0
```

The ```method``` is selected according to the following table.

| Code | Method                                                                                     |
|------+--------------------------------------------------------------------------------------------|
| 0    | [Helmholtz EPT](methods/ept-helmholtz)                                                     |
| 1    | [Convection-reaction EPT](methods/ept-convreact)                                           |
| 2    | [Gradient EPT](methods/ept-gradient)                                                       |
| 100  | [Phase-based Helmholtz EPT with automatically selected kernel](methods/ept-helmholtz-chi2) |

Starting from the code 100, the implemented methods are still experimental.

## Mesh

```toml
[mesh]
    size = [128,128,8]
    step = [1e-3,1e-3,4e-3] # [m]
```

- ```size``` is the number of voxels of the input data in each direction.
- ```step``` is the size of a voxel in meters.

## Input

```toml
[input]
    frequency = 64e6 # [Hz]
    tx-channels = 2
    rx-channels = 4
    tx-sensitivity = "phantom.h5:/tx_sens>"
    trx-phase = "phantom.h5:/trx_phase><" # [rad]
    wrapped-phase = false
```

- ```frequency``` is the Larmor frequency of the input data in hertz.
- ```tx-channels``` is the number of transmit channels in the input data.
- ```rx-channels``` is the number of receive channels in the input data.
- ```tx-sensitivity``` is the address of the transmit sensitivity (magnitude). It must be a dataset in an .h5 file.
- ```trx-phase``` is the address in an .h5 file of the transceive phase in radians. It must be a dataset in an .h5 file.
- ```wrapped-phase``` is equal to ```true``` if the input transceive phase maps are wrapped. Currently, only the phase-based methods take advantage of it. (default: ```false```).

For some EPT methods, the ```tx-sensitivity``` or the ```trx-phase``` could be optional.

In case of multiple transmit channels, the wildcard character ```>``` is used in the ```tx-sensitivity``` address in order to distinguish the data from each channel. It will be replaced by an increasing integer number from zero up to the number of transmit channels minus one.

Similarly, multiple receive channels are distinguished in the ```trx-phase``` address by means of the wildcard character ```<```, which will be replaced by an increasing integer number from zero up to the number of receive channels minus one.

For example, with the previous input settings the transmit sensitivity data would be read from \\
```"phantom.h5:/tx_sens0"``` \\
```"phantom.h5:/tx_sens1"``` \\
whereas the transceive phase data would come from \\
```"phantom.h5:/trx_phase00"``` ```"phantom.h5:/trx_phase10"``` \\
```"phantom.h5:/trx_phase01"``` ```"phantom.h5:/trx_phase11"``` \\
```"phantom.h5:/trx_phase02"``` ```"phantom.h5:/trx_phase12"``` \\
```"phantom.h5:/trx_phase03"``` ```"phantom.h5:/trx_phase13"```

### Wildcard characters

It is possible to change the default wildcard characters, the starting index and the increasing step with the following optional instructions in the .toml configuration file.

```toml
[input.wildcard]
    tx-character = '>'
    rx-character = '<'
    start-from = 0
    step = 1
```

## Output

```toml
[output]
    electric-conductivity = "example.h5:/sigma" # [S/m]
    relative-permittivity = "example.h5:/epsr"
```

- ```electric-conductivity``` is the address where the electric conductivity, expressed in siemens per meter, will be written. It must be a dataset in an .h5 file.
- ```relative-permittivity``` is the address where the relative permittivity will be written. It must be a dataset in an .h5 file.

If an output address is not provided in the configuration file, the corresponding properties evaluation will not be stored.

## Parameters

In addition to the previous settings, that are common to all EPT methods, each method could require some additional parameters.
The settings for each specific EPT method are described in their pages, where examples of configuration files are also provided (see [Methods](methods)).
