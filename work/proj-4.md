---
title: Topology Stuff
---

*This algorithm is discussed at length in [my dissertation]({{ site.github.url
}}/assets/TKDissertation.pdf).*

In multiphase-field datasets, grains are regions in which one order parameter
is "on" and the rest are "off." The grain boundaries are defined implicitly as
those regions in-between where two or more order parameters are partially "on."

{: .imgcenter}
![Diffuse]({{ site.github.url }}/assets/img/work/proj-4/Profiles.png)
*2D multiphase-field data with grains as solid-colored regions and exaggerated
grain boundaries in-between. Line scans show quantitative differences between
bulk grain interior, edges, and vertices in the network.*

Picking out grains is trivial for the human eye, but challenging for a computer:
the features of the grain boundary (vertices, edges, and facets) are ambiguous.
To get started, vertices are interpreted as the lowest points in the magnitude
of the data, |&phi;|. From each of these points, a fast-marching level set
method propagates the weighted distance back throughout the local network.

{: .imgcenter}
![Propagation]({{ site.github.url }}/assets/img/work/proj-4/propagation.png)
*Propagation of distance to the central vertex (in black) through the local
grain boundary network using a fast-marching level set method. This process
repeats for each vertex in the domain, recording a vector-valued distance
map.*

This produces a vector-valued map of the distance from any point to every
nearby vertex. This map is used to traverse from each vertex to every possible
neighbor, building a vertex adjacency matrix in the process. If a trial edge
between vertex *s* and vertex *t* encounters vertex *m*, then the edge *s-t*
must be rejected. Detection of these intermediate vertices is limited by the
interface width of the simulation data. When two vertices fall within roughly
one interfacial width apart, they are indistinguishable. Practically, this only
occurs in the transient before steady-state and when an edge collapses.

{: .imgcenter}
![Near miss]({{ site.github.url }}/assets/img/work/proj-4/nearmiss.png)
*Due to the diffuse interfaces -- outlined in black -- inherent to phase-field
data, a trial edge (in red) can easily "miss" an intermediate vertex **m** as it
traverses from **s** to **t**. The algorithm prevents this for vertices separated
by distances greater than the interfacial width. This resolution is much better
than more simplistic algorithms.*

Traversing from each vertex to its possible neighbors eliminates false edges.
Next, chordless cycles of the graph are identified and filtered to determine
the faces of each grain. Consider the graph representation of a cube, with
each cube corner given a letter:

{: .imgcenter}
![chordless]({{ site.github.url }}/assets/img/work/proj-4/cycles.png)
*Chordless cycles of the cube, with corners labeled A--H. The six faces
are represented by four-letter cycles.*

From the mass of edges emanating from each grain, constructing chordless cycles
provides a convenient means to filter out edges that terminate in a neighboring
grain. An additional filter using the vector-valued data -- that is, the phases
present at the origin vertex for each cycle -- helps filter cycles that, while
chordless, represent meanders around the perimeter rather than valid faces of
a grain.

At the end, this multistage process -- detect "low points," map distances, find
chordless cycles -- results in a global set of topological features: vertices,
edges, and facets. Since the phases present at each point are known from the
underlying phase-field data, individual grain topologies are readily constructed
from the global dataset.

These topological representations -- local and global -- are useful in analyzing
characteristics of the polycrystal. Since my interest was in tracking triangles,
I record the topology of each grain in a compact way. This is done by
extracting the vertex-adjacency matrix for a grain from the global matrix, then
traversing the grain-graph to produce its Weinberg vector.

{: .imgcenter}
![Weinberg]({{ site.github.url }}/assets/img/work/proj-4/weinberg.png)
*Construction of two string representations of grain topology using
vertex adjacency data. After alphabetically sorting the vectors produced from
every vertex, the first one is taken as the unique Weinberg vector. For this
five-faced graph, it is ABCACDEAEFBFDFEDCBA.*

This representation can then be used for tracking topological changes as grains
grow, or to compare the most frequently observed topologies against published
polycrystal datasets, or for examining unusual features of the grain boundary
network.

Source code for extracting grain topologies from sparse multiphase-field data
is distributed with [MMSP](https://github.com/mesoscale/mmsp) as the [mmsp2topo
utility](https://github.com/mesoscale/mmsp/blob/master/utility/mmsp2topo.cpp).
