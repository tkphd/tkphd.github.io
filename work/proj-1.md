---
title: Dissertation Stuff
---

### Grain Boundary Network Topology (Dissertation)

***
For a detailed discussion of this work, please refer to my dissertation,<br>
[Bias in Polycrystal Topology Caused by Grain Boundary Motion by Mean Curvature.]({{ site.github.url }}/assets/TKDissertation.pdf)

***

Metals are not uniform blocks of material, but instead are composed of tiny subdomains of common crystalline orientation called "grains."
They are visible in the zinc coatings of galvanized steel guard rails, or in this polished and etched nickel alloy:

{:.imgcenter}
![grains in Monel 400]({{ site.github.url }}/assets/img/work/proj-1/monel.png)
*Grains in polycrystalline nickel scatter light differently due to crystal orientations.*

Grains grow, especially at elevated temperatures. Each interface grows toward its center of curvature, and grains with net outward curvature
grow at the expense of neighbors with net inward curvature. In 2D, this produces the "*n* minus 6" rule:

{:.imgcenter}
![grain curvature predicts growth]({{ site.github.url }}/assets/img/work/proj-1/unstable.png)
*Stable 2D junctions make 120&deg; angles: hexagons are stable, while  heptagons and above grow at the expense of pentagons and below.*

This process of *motion by mean curvature* tends to eliminate three-edged faces in 3D polycrystals.
When comparing the topology of grains from natural polycrystals to mathematically derived space-filling aggregates,
such as Voronoi tessellations, the keen observer will notice a lot more 14-faced grains in the natural polycrystal
than in the mathematical construct. This suggests bias in the process of grain growth.
My dissertation work focused on this process, and the effects of triangle elimination on the topology
of the grain boundary network in 2D and 3D. Since metal is opaque, I turned to numerical simulations
to produce large grain populations for analysis.

I chose [phase-field simulations](https://en.wikipedia.org/wiki/Phase_field_models), since their foundations
in statistical thermodynamics allow for realistic physical features in the model. The downside is that the
interfaces are diffuse, not sharp, so I created an algorithm to extract discrete grain boundary
networks from 2D and 3D datasets:

{:.imgcenter}
![2D network]({{ site.github.url }}/assets/img/work/proj-1/gbnetwork.png)
*Illustration of diffuse boundaries (left) and discrete network reconstruction (right) from 2D data.*

{:.imgcenter}
![3D network]({{ site.github.url }}/assets/img/work/proj-1/gbgraph.png)
*Discrete grain boundary network in 3D. The reconstruction produces a complete adjacency matrix for the grain boundary network graph.*

The phase-field model provided an easy means of changing the physics of grain growth. The most pronounced effects on topology of the
full grain boundary network arose from reducing the mobility of the diffuse interface near triple junctions.
These are vertices in 2D or edges in 3D, and "pinning" them tends to straighten the adjacent edges and facets.

{:.imgcenter}
![trijunction drag]({{ site.github.url }}/assets/img/work/proj-1/tjpinning.png)
*2D polycrystals grown under ideal conditions (left) and with reduced mobility at triple junctions (right) from an identical initial condition. Note overall straightening of edges.*

This straightening of edges dampens motion by mean curvature, thereby prolonging the lifetimes of triangular grains.
This effect demonstrates that *motion by mean curvature is the cause of bias in polycrystal topology.*

<style>
.imgcenter {
    text-align:center;
}
</style>
