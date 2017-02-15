---
title: Parallel Stuff
---

### Parallel Programming

Software for solving partial differential equations on spatially discretized
meshes are inherently parallelizable: integration in time requires data from a
few neighboring points, but in most cases, long-range effects are negligible.
Therefore, the full simulation domain can be subdivided into smaller units.

{: .imgcenter}
![ghosts]({{ site.github.url }}/assets/img/work/proj-3/thumb.png)
*Spatial decomposition of the system into parallel subdomains.*

In a shared-memory architecture, *e.g.* using all the cores of a modern CPU,
no further effort is needed. The workload can be partitioned among POSIX
threads or using the enormously useful OpenMP library. In distributed-memory
architectures, *e.g.* using many separate computers in a cluster, some
computations require data from neighboring subdomains. These are stored in
"ghost cells" along the boundary, which must be synchronized to prevent the use
of stale data.

{: .imgcenter}
![Ghosts]({{ site.github.url }}/assets/img/work/proj-3/ghosts.png)
*Illustration of a 2D domain subdivided among three processors with distributed
memory. Spatial operators, such as finite differencing at point (i,j), require
data from "ghost" cells in gray and local cells in black.*

Exchanging this boundary data is typically done with MPI, the message passing
interface. Many programs use the simplest commands, ```MPI_Send``` and
```MPI_Recv```, to exchange chunks of data between parallel processes. These
are *blocking* operations: a process which enters ```MPI_Send``` will not
return until the data has been sent to the destination process, and likewise
for ```MPI_Recv```. Such operations run a high risk of *deadlock* when the send
and receive operations occur in the wrong sequence. When scaling a parallel
program beyond a couple of processors, it is important to use the non-blocking
interfaces, ```MPI_Isend``` and ```MPI_Irecv```, followed by ```MPI_Wait``` to
make sure the commands succeed.

Phase-field simulations using [MMSP](https://github.com/mesoscale/mmsp) on an
IBM Blue Gene/Q supercomputer were only possible after I overhauled its grid
class to use the non-blocking interface to each MPI function call. However,
the initial condition data files -- much smaller than subsequent checkpoints --
were always corrupt. As it turns out, the [parallel filesystem](
https://en.wikipedia.org/wiki/IBM_General_Parallel_File_System) block size
(8MB) was larger than the datafile, so every parallel process was attempting to
write to the same location on disk. The solution, now integrated into MMSP, is
to perform parallel writes in two stages: just one MPI process is assigned to
write each block, and allocates a buffer to store one block's worth of data.
The remaining majority of processes send data to the appropriate writer,
dividing data as needed to make a fully dense file with bits in the correct
sequence. Once the gather operation completes, the writers write to disk.

{: .imgcenter}
![Parallel]({{ site.github.url }}/assets/img/work/proj-3/gatherio.png)
*Schematic of parallel gather-and-write algorithm to prevent filesystem block
collisions. On the left, each of the 8 MPI processes **r0**--**r7** writes to
disk, with high risk of corruption. On the right, only ranks **r0**, **r3**,
and **r7** write -- one per block. The rest send data with byte offset markers
to guarantee a dense file with the correct sequence of data on disk.*

### Software Testing

Using an online continuous integration tool, such as [Travis CI](
https://travis-ci.org/), is perhaps the easiest way to check proposed software
changes for errors. Essentially, whenever new code is committed to the
repository, the continuous integration service runs a test script and reports
success or failure. For public online repositories, these tests can also run
on proposed changes in pull requests.

Here are a few of the projects I have configured for automatic testing using
[Travis CI](https://travis-ci.org/tkphd):

| [ trevorkeller.com ](https://github.com/tkphd/tkphd.github.io)                         | | [ ![Build Status](https://travis-ci.org/tkphd/tkphd.github.io.svg?branch=master) ](https://travis-ci.org/tkphd/tkphd.github.io) |
| [ MMSP examples ](https://github.com/mesoscale/mmsp)                                   | | [ ![Build Status](https://travis-ci.org/mesoscale/mmsp.svg?branch=master) ](https://travis-ci.org/mesoscale/mmsp) |
| [ MMSP benchmark ](https://github.com/mesoscale/MMSP-spinodal-decomposition-benchmark) | | [ ![Build Status](https://travis-ci.org/mesoscale/MMSP-spinodal-decomposition-benchmark.svg?branch=master) ](https://travis-ci.org/mesoscale/MMSP-spinodal-decomposition-benchmark) |
| [ FiPy benchmark ](https://github.com/usnistgov/FiPy-spinodal-decomposition-benchmark) | | [ ![Build Status](https://travis-ci.org/usnistgov/FiPy-spinodal-decomposition-benchmark.svg?branch=master) ](https://travis-ci.org/usnistgov/FiPy-spinodal-decomposition-benchmark) |
