---
layout: default
title: Home
nav_order: 1
description:
permalink: /
last_modified_date: 2020-07-30T10:40:39+02:00
---

![](/assets/images/logo-eptlib.png)
{: .text-center }

# EPTlib v0.2.0
{: .fs-9 }

EPTlib is an open source, extensible collection of C++ implementations of electric properties tomography (EPT) methods.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/eptlib/eptlib){: .btn .fs-5 .mb-4 .mb-md-0 target="_blank" }

---

## Getting started

The binaries of EPTlib for different operative systems can be downloaded [here](https://github.com/EPTlib/eptlib/releases).

Alternatively, EPTlib can be built directly from its [source](https://github.com/EPTlib/eptlib).

## The EPTlib application

The EPTlib application allows applying the implemented EPT methods on input data.
It is a terminal application that can be run by typing

```bash
EPTlib_app <filename>.toml
```

where ```<filename>``` must be replaced with the name of the configuration file for the case of interest.

Details on the set-up of the configuration file are provided in the [Settings](settings) page.

## List of implemented methods

The following methods are currently implemented in EPTlib and can be executed from the EPTlib application:

1. [Helmholtz EPT](methods/ept-helmholtz)
1. [Convection-reaction EPT](methods/ept-convreact)
1. [Gradient EPT](methods/ept-gradient)

---

## About the project

EPTlib is &copy; 2020-{{ "now" | date: "%Y" }} by [Alessandro Arduino](http://github.com/alessandroarduino), INRiM.

[![](/assets/images/logo-inrim.png)](https://www.inrim.eu){: .mb-4 .mb-md-0 target="_blank" }

### License

EPTlib is distributed by an [MIT license](https://github.com/eptlib/eptlib/tree/master/LICENSE).

### Acknowledgement

EPTlib has been developed in the framework of the 18HLT05 QUIERO Project. This project has received funding from the EMPIR Programme, co-financed by the Participating States and from the European Union's Horizon 2020 Research and Innovation Programme.

[![](/assets/images/logo-empir-euramet.png)](https://www.euramet.org/research-innovation/research-empir/){: .mb-4 .mb-md-0 .mr-2 target="_blank" }
[![](/assets/images/logo-quiero.png)](https://quiero-project.eu){: .mb-4 .mb-md-0 target="_blank" }
