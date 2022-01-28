## Intro 

PIC or particle-in-cell simulation is a technique to solve a certain class of partial differential equations. In this method, the individual particles (or fluid elements) are tracked in continuous phase space while the moments of distribution such as the densities, the currents (field-derived quantities) are computed simultaneously on a mesh grid.

In plasma physics applications, the method is used to follow the trajectories of the charged particles in self-consistent EM fields computed on a fixed mesh. Procedure of a PIC simulation in the plasma physics context

1. Integration of the equation of motion of charges.
2. Interpolation of the charge and current source terms to the field mesh.
3. Computation of the fields on mesh points.
4. Interpolation of the fields from the mesh to the particle locations.

## Basics of PIC plasma simulation
PIC codes include
1. **pusher** or **particle mover** which solves the Lorentz force as the equation of motion.
2. **field solver** solves the Maxwell's equations to determine the electric and magnetic fields.

### Super-particles
A real system consists of extremely large number of particles. To efficiently simulate a real system, PIC uses the so-called **super-particles**. A super-particle a.k.a macroparticle is a computational particle that represents many real particles. For example, it might be millions of electrons or ions in the case of a plasma  simulation, or a single vortex element in a fluid simulation. Because the Lorentz force only depends on the charge-to-mass ratio, the super-particle follows the same trajectory as represented real particle would. 

The number of real particles corresponding to a super-particle must be chosen such that sufficient statistics can be collected on the particle motion. 

### The particle mover
Even with super-particles, the number of simulated particles is usually very large (in the order of millions). The particle mover, therefore, is the most time-consuming part of a PIC simulation. The pusher is required to be very precise and fast. Much effort has been spent on optimizing different pusher schemes. Two main categories of the pusher schemes are the **implicit** and **explicit** solvers. 
- The implicit solvers use the already updated fields to calculate the velocity of the particles,
- while the explicit solvers only use the field values from the previous time step. The explicit solvers are simpler and faster but requires smaller time step.
- **Leapfrog method** - a second-order explecit method - are also used.
- **Boris algorithm** is used to cancel out the magnetic field in the Newton-Lorentz equation. Because of its excellent long term accuracy, Boris algorithm is the de-facto standard for advancing the charged particles in a plasma. 

### The field solver
The most common methods for solving the Maxwell's equations or more generally any PDE belongs to one of these three categories
1. **FDM** finite difference method.
2. **FEM** finite element method.
3. Spectral method.

In FDMs, the continuous domain is discretized with a grid of points, on which the electric and magnetic fields are calcualated. Their derivatives are approximated with the differences between neighboring points on the grid. This way, the Maxwell's equations turn into algebraic equations.

IN FEMs, the continuous domain is divided into discrete mesh of elements. The PDEs are treated as an eigenvalue problem and initially a trial solution is evaluated using a set of basis functions that are localized in each element. The final solution is the obtained by an optimization process untill the required accurary is attained.

In spectral methods (e.g. fast Fourier transform), the PDEs are transformed into an eigenvalue problem. However, this time, the basis functions are high order and defined globally over the whole domain. Then domain itself stays continuous. Trial solutions are found by inserting the basis functions into the eigenvalue equation and then optimized to determine the best values of the initial trial parameters.

### Particle and field weighting 
The name *particle-in-cell* originates in the way that plasma macro-quantities (number density or current density) are assigned to the simulated particles (weighting). Particles can be situated anywhere on the continuous domain, but the fields only get evaluated on the mesh points. To obtain the macro-quantities, we assume that the particles have a given **shape** determined by the shape function. 
