#####
CVMix
#####

The Community Vertical Mixing Project (CVMix) created a library of first-order closures to
parameterize vertical mixing in ocean models. CVMix is used in POP2, MOM6 and MPAS ocean.

Installation instructions (for M2 MacBook Pro using MacPorts):

.. code-block::

   git clone https://github.com/CVMix/CVMix-src.git
   cd CVMix-src/src

Running make will invoke an executable, ``cvmix_setup``, that will configure the makefile.

.. code-block::

   make
   Fortran compiler (mpi not necessary): gfortran
   Directory containing netcdf configuration tool nc-config (or enter "no-nc-config" to    enter location of netcdf include and netcdf lib directories): /opt/local/bin
   [ messages from make ]

The resulting library file will be compiled here:

.. code-block::

   CVMix-src/lib/libcvmix.a
