---
title: Parallel Stuff
---

Software for solving partial differential equations on spatially discretized
meshes are inherently parallelizable: integration in time requires data from a
few neighboring points, but in most cases, long-range effects are negligible.

{: .imgcenter}
![Parallel]({{ site.github.url }}/assets/img/work/proj-3/thumb.png)
*Spatial decomposition of the system into parallel subdomains.*

{: .imgcenter}
![Parallel]({{ site.github.url }}/assets/img/work/proj-3/img1.png)
*Schematic of parallel gather-and-write algorithm to prevent filesystem block collisions.*

Software Testing:

| [MMSP examples](https://github.com/mesoscale/mmsp)                                   | [![Build Status](https://travis-ci.org/mesoscale/mmsp.svg?branch=master)](https://travis-ci.org/mesoscale/mmsp) | [![OpenHub](https://www.openhub.net/p/mmsp/widgets/project_thin_badge.gif)](https://www.openhub.net/p/mmsp) |
| [MMSP benchmark](https://github.com/mesoscale/MMSP-spinodal-decomposition-benchmark) | [![Build Status](https://travis-ci.org/mesoscale/MMSP-spinodal-decomposition-benchmark.svg?branch=master)](https://travis-ci.org/mesoscale/MMSP-spinodal-decomposition-benchmark) | |
| [FiPy benchmark](https://github.com/usnistgov/FiPy-spinodal-decomposition-benchmark) | [![Build Status](https://travis-ci.org/usnistgov/FiPy-spinodal-decomposition-benchmark.svg?branch=master)](https://travis-ci.org/usnistgov/FiPy-spinodal-decomposition-benchmark) | |
| [tkphd.github.io](https://github.com/tkphd/tkphd.github.io)                          | [![Build Status](https://travis-ci.org/tkphd/tkphd.github.io.svg?branch=master)](https://travis-ci.org/tkphd/tkphd.github.io) | |
