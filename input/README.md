# 0. Input for the simulation

## 0.1. ASCII format

This format is supposed to be used in case if an external event generator provides a list of reaction products with particle ID, momentum and start position (vertex).

### Preparing an input file

The input file is described event-wise with one line of a header, followed by lines describing the content, one entry per primary particle. The structure is as following:

~~~
eventNumber  multiplicity  0.  0.
flag  Z  A  px  py  pz  vx  vy  vz  mass
~~~

* eventNumber - index of an event, usually starts at 0
* multiplicity - number of primary particles, which should match number of lines describing those particles
* “0.  0.” - not used, but has to be in the header
* flag - 1 if an elementary particle (described with PDG code) or -1 - if a fragment
* Z - 0 if an elementary particle or charge in case of fragment
* A - PDG code for a particle or A for a fragment
* px py pz - total momentum (LAB) in GeV/c
* vx vy vz - start position (vertex) in cm
* mass - mass of a particle in GeV/c2

### Example

Below is an example of an event with 1 fragment and 4 neutrons:

~~~
0     5   0.   0.
 -1   50      128     -0.0024691    0.0197320  155.5612335    0.00  0.00  0.00   127.910537
  1    0     2112      0.0023880   -0.0037039    1.2324538    0.00  0.00  0.00   0.939565379
  1    0     2112      0.0074641    0.0036909    1.2642491    0.00  0.00  0.00   0.939565379
  1    0     2112     -0.0062601   -0.0140930    1.2316210    0.00  0.00  0.00   0.939565379
  1    0     2112     -0.0011230   -0.0056258    1.2183754    0.00  0.00  0.00   0.939565379
~~~

### Usage

Files are to be placed in __R3BRoot/input__ directory. To use in simulation, in the file __r3bsim.C__ _line 40_:

~~~
TString fGene = "ascii";
~~~

and _line 113_:

~~~
OutFile, ParFile, "Name_Of_the_Input_File", 335566);
~~~

The filename should be relative to R3BRoot/input directory.

## 0.2. Box generator

The so called "box" generator creates particles with uniform distribution in phase-space (4-momentum). It can be used for generic simulations like acceptance studies or efficiency calculations.

### Example

The box generator is created and configured directly in a simulation macro using the __FairBoxGenerator__ class. The following example shows how to generate 3 protons per event (PDG code and multiplicity in the standard constructor of the class), with polar angle between 0 and 2 degrees (method SetThetaRange), momentum in a range 1.3 - 1.5 GeV/c (SetPRange), and isotropic phi distribution (SetPhiRange). The method SetXYZ sets the starting coordinates for the particles. The instance of the box generator is added to the primary generator.

~~~cpp
FairBoxGenerator* boxGen = new FairBoxGenerator(2212, 3);
boxGen->SetThetaRange(0., 2.);
boxGen->SetPRange(1.3, 1.5);
boxGen->SetPhiRange(0, 360);
boxGen->SetXYZ(0.0, 0.0, -1.5);
primGen->AddGenerator(boxGen);
~~~

### Usage

There can be multiple box generators used in one simulation run. All instances are to be added to one primary generator.

To use in simulation, in the file __r3bsim.C__ _line 40_:

~~~
TString fGene = "box";
~~~

and the particles description can be changed in the file __r3ball.C__ _line 295_.

## 0.3. Ion generator

...