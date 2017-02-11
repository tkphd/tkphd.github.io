---
title: Additive Stuff
---

*Micrographs for this section will be posted after my upcoming paper,
"Application of Finite Element, Phase-field, and CALPHAD-based Methods
to Additive Manufacturing of Ni-based Superalloys", goes to press.
For a discussion of precipitate phases found in Inconel 625, take a
look at my colleagues' paper, ["Homogenization kinetics of Inconel
625 produced by powder bed fusion laser sintering"](
https://dx.doi.org/10.1016/j.scriptamat.2016.12.037).*

Additive manufacturing of metal parts usually introduces microscopic patterns
of chemical non-uniformity, which later heat treatments are intended to fix.
In some cases, however, heat treatment provides the activation energy needed
by secondary phases to precipitate and grow into finely dispersed particles,
shown schematically below.

{: .imgcenter}
![Precipitate]({{ site.github.url }}/assets/img/work/proj-2/thumb.png)
*Schematic composition map showing precipitates within enriched interdendritic
region. This is a proposed initial condition for solid-state simulations.*

In X-ray diffraction patterns and transmission elecron micrographs of heat-treated
Inconel 625 material produced by laser-powder bed fusion, I have noticed three
types of metallic precipitate: &delta;, Laves, and, rarely, &mu;. For a Ni-rich
alloy like Inconel 625, these phases are all represented in the Cr--Nb--Ni ternary
system, an approximate phase diagram for which is presented below.

{: .imgcenter}
![Ternary Diagram]({{ site.github.url }}/assets/img/work/proj-2/ternary.png)
*Ternary phase diagram for Cr-Nb-Ni at 870&deg;C, generated from CALPHAD data
using Qhull. Lines between phases are triangular facets of the convex hull,
not thermodynamic tie lines.*

While the &mu; phase is present, in blue at the bottom-right, it can not exist
in equilibrium with the matrix (&gamma;) phase, in red along the left edge. The
observed &mu; particles must collapse, feeding one of the other precipitate
phases or dissolving into the matrix.

Since this is an equilibrium diagram, it is not clear how long this process will
take. In order to help guide process engineering of the heat treatments, I am
using a phase field model with three components (Cr, Nb, and Ni) and four
phases (&gamma;, &delta;, Laves, and &mu;) fed by a CALPHAD (CALculation of
PHAse Diagrams) database. This should give a thermodynamically accurate
prediction of the particle aging (or "reversion") process in this material.

Source code for the model, as well as a detailed mathematical description,
are available in the [GitHub repository](
https://github.com/usnistgov/phasefield-precipitate-aging).
