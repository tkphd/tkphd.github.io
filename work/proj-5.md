---
title: Numerical Stuff
---

During the first [CHiMaD Phase Field Hackathon](
https://pages.nist.gov/pfhub/hackathons/hackathon1/problems.ipynb/), I solved
the problem using [MMSP](https://github.com/mesoscale/mmsp) with an explicit
solver modeled after the [Cahn-Hilliard example](
https://github.com/mesoscale/mmsp/tree/master/examples/phase_transitions/cahn-hilliard/explicit).
Afterwards, I was curious about more sophisticated numerical methods and wound
up coding a semi-implicit version based on [convex splitting and Gauss-Seidel](
https://github.com/mesoscale/mmsp/tree/master/examples/phase_transitions/cahn-hilliard/convex_splitting)
This technique guarantees a monotonic decrease in system free energy, no matter
what timestep is chosen. To test the code, I ran a large number of simulations
to explore the parameter space, varying timestep and relaxation parameter, and
analyzed the variations in mass and number of iterations.

{: .imgcenter}
![CahnHilliardMass]({{ site.github.url }}/assets/img/work/proj-5/masscontours.png)
*Contour plot of final system mass as a function of timestep and relaxation
parameter. With zero-flux boundary conditions, mass is exactly conserved in an
explicit code; any deviation is physically incorrect. Black points succeeded, red points
failed to converge.*

{: .imgcenter}
![CahnHilliardIter]({{ site.github.url }}/assets/img/work/proj-5/workcontours.png)
*Contour plot of total number of iterations to reach target simulation time.
Red line (0.45e6 iterations) represents parity with the explicit code. Black
points succeeded, red points failed to converge.*

The data show that tolerable deviations in mass (outlined by the green isotherm)
were only achieved with the same number of iterations as the explicit code: in
other words, there was no performance benefit in using the more advanced (and
complex) numerical method for this problem.

Coding this semi-implicit solver by hand gave me an opportunity to become very
familiar with the inner workings of Gauss-Seidel and iterative methods. It also
gave me an appreciation for numerical libraries, which offer convenient
interfaces to optimized implementations of many common algorithms.

Source code for the spinodal decomposition problem using convex splitting and
Gauss-Seidel with successive over-relaxation is available as an [MMSP example](
https://github.com/mesoscale/mmsp/tree/master/examples/phase_transitions/cahn-hilliard/convex_splitting).

***

My next project involved a phase-field model for solidification from a molten
binary alloy, with solid-liquid interfaces modeled using the [Kim-Kim-Suzuki](
https://doi.org/10.1103/PhysRevE.60.7186) model. This requires an iterative
solver to find the roots of a system of equations in two dimensions. To solve
it, I called [GSL](
https://www.gnu.org/software/gsl/manual/html_node/Multidimensional-Root_002dFinding.html)
from [MMSP](https://github.com/mesoscale/mmsp) and [SciPy](
https://docs.scipy.org/doc/scipy-0.14.0/reference/optimize.html) from [FiPy](
https://github.com/usnistgov/fipy).

{: .imgcenter}
![KKSenergy]({{ site.github.url }}/assets/img/work/proj-5/KKSenergy.png)
*Free energy landscape for binary solidification model showing trajectories
through the interface as a planar growth front equilibrates.*

While this solver would be easy to code from scratch, the scientifically
[interesting model](https://github.com/usnistgov/phasefield-precipitate-aging)
requires an 8-dimensional root solver. The wrapper class to handle GSL function
calls translated easily to more dimensions, and allowed for trivial changes
to switch between Newton's Method and gradient descent algorithms.

Source code for binary solidification of Cu--Ni using KKS interfaces and a CALPHAD
free energy database, implemented side-by-side in [MMSP](https://github.com/mesoscale/mmsp)
and [FiPy](https://github.com/usnistgov/fipy),
is available on [GitHub](https://github.com/tkphd/KKS-binary-solidification).
