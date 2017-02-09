---
title: Parallel Stuff
---

Software for solving partial differential equations on spatially discretized
meshes are inherently parallelizable: integration in time requires data from a
few neighboring points, but in most cases, long-range effects are negligible.

{:.imgcenter}
![Parallel]({{ site.github.url }}/assets/img/work/proj-4/thumb.png)
*Spatial decomposition of the system into parallel subdomains.*

{:.imgcenter}
![Parallel]({{ site.github.url }}/assets/img/work/proj-4/img1.png)
*Schematic of parallel gather-and-write algorithm to prevent filesystem block collisions.*


<style>
.imgcenter {
    text-align:center;
}
</style>
