# EM-CG_tutorial

## Assignment 1 - LJ-particles phase diagram
As a starting exercise in coarse-grained molecular dynamics, let's explore the phase diagram of the simplest possible coarse-grained objects: Lennard-Jones particles [Lennard-Jones fluid](https://en.wikipedia.org/wiki/Lennard-Jones_potential).

### Instructions

2a. Take the `in.3dlj` script, using a cutoff of 2.5 for the Lennard-Jones potential. We placed question marks for the parameters you need to change. If you try to run the script as is, it will throw an error. Look for comments with `!!!!!!!!!!` to locate those parameters and adjust accordingly.

(i) Fix the temperature to 1.0, and vary the particle density and interaction strength. Report snapshots and quantitative metrics (on structure and/or dynamics) for different phases of the system, including at least a liquid phase and a crystalline phase. You can use the "common neighbor analysis" in Ovito to identify different phases.

> **Pro tip:** Equilibrate long enough, and stay far away from the phase boundaries unless you want to wait for a long time.


## Assignment 2 - Coarse-graining fullerene

The key for building new CG models is to calibrate the definition and interactions of your CG beads, either with bottom-up (e.g. from AA simulations) or top-down (e.g. from experimental results) approaches. For simple spherical particles, a common approach is to calculate the [Potential of Mean Force (PMF)](https://en.wikipedia.org/wiki/Potential_of_mean_force) between two "particles". Let's do that for fullerene in vacuum!

### Instructions

1a. After visualizing the c60-bonds.data file, run the `in.fullerene` simulation. Create an empty directory `lmp_data/` in the same directory as the script.

(i) What is the role of the looping structure in this simulation?

(ii) From the thermodynamic information in the log file/the trajectory, extract two plots: the force and the potential energy between the two molecules as a function of the distance between their centers of mass.

> **Hints for the PMF calculation:** We save the forces acting on every atom (fx, fy, fz). Identify different molecules in the simulation using MDAnalysis (what does `u.atoms.fragments` do?). You can access the forces on particles just like positions: `u.atoms.forces`. Sum the forces over all atoms in each molecule to get the total force vector on molecule 1 and molecule 2. Compute the centre of mass of each molecule, then the vector connecting them. Project the force on one molecule onto this vector (why?). Store the distance and projected force for each frame. Then average the forces in distance bins and integrate using `scipy.integrate` (we let you decide which function to use) to get the PMF.

1b. Time to coarse-grain!

(i) Fit your potential energy plot with an interatomic potential function of your choice and report the values of the fitting parameters. 

(ii) How would you now define a simulation of coarse-grained particles interacting with your effective potential? Specify all the information you would need to set it up. 

(iii) (OPTIONAL) actually set up and run such a coarse-grained simulation, and compare the results of the CG and AA models (for example comparing the radial distribution function peak positions for the centers of mass).

## Assignment 3 - Coarse-graining Polylactic Acid

Develop a coarse-grained model for a simple polymer (polylactic acid) from a higher-resolution all-atom model. The complex macromolecular structure means that the chemical connectivity must be considered by using multiple beads with bonded interactions. For both bonded and non-bonded interactions, we follow a coarse-graining scheme called (Iterative) Boltzmann Inversion. With number of iterations N=1...

### Instructions

1a. Load the `PLA_CHARMM.data` atomistic structure in Ovito and inspect it. How many atoms belong to the same monomer? How many monomers are present in the entire chain?

1b. Define the bonded interactions of the CG model.

(i) Run the script PLA_CHARMM.in of a single atomistic chain.

(ii) From the sampled configurations, use the scripts in the `prob_distr_calc/` directory to plot the distributions of all bonds, angles, and dihedrals between the centers of mass of the CG beads. The scripts plot the distributions for the "effective" CG beads from the all-atom simulations, fit them to an analytical function and output the fitting parameters. How does MDanalysis identify the CG beads? Tip: look into the compute_bond_distances() function in the compute_bonds.py file.

(iii) From your fits, write the equations used for the bonded potential terms of the CG force field, specifying the values of all parameters obtained. Discuss briefly also what you would have to do to then calibrate the non-bonded interactions.

1c (OPTIONAL, EASY BUT TAKES SOME TIME). Calibrate the non-bonded interactions to complete the CG force field. Run both AA and CG simulations of several chains (density ~0.9, temperature 300K), and compare the distributions of bonds, angles, and radial distribution function between the AA and the CG models.
