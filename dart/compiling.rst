#########
Compiling
#########

For the ASD project, DART will be compiled with the PrgEnv-gnu programming 
environment and compilers recommended by HPE/Cray.

During the NERSC December 2021 GPU Hackathon, Johnson, along with DAReS
Software Engineer Helen Kershaw and ASP Post-doc Chris Riedel, ported DART
code to the Department of Energy's Perlmutter machine, which is a Cray EX
supercomputer (Derecho is also a Cray EX). Under the guidance of three
development engineers from Nvidia, the team GPU-accelerated a kernel of DART's
source code using Nvidia's nvfortran compiler and OpenACC directives.
The code was accelerated by a factor of 15x using pinned memory, but the
computed results were incorrect (there is a possible "off by one" bug in the
compiler). Thus this project does not plan to use the PrgEnv-nvidia compilers.

