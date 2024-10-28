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

cvmix_data_type
===============

The ``cvmix_data_type`` is a derived type containing time-dependent variables
for each column in the domain. The type definition is defined in
``src/shared/cvmix_kinds_and_types.F90``.

It contains scalar variables such as integer-valued:

- ``nlev``

and real-valued:

- ``OceanDepth``
- ``BoundaryLayerDepth``
- ``lat``
- ``lon``
- ``Coriolis`` (Coriolis parameter), etc.

It contains many vector-valued variables corresponding to values at cell
centers or cell interfaces. Some examples are diffusivity coefficients at 
interfaces:

- ``Mdiff_iface`` (momentum diffusivity)
- ``Tdiff_iface`` (temperature diffusivity) and
- ``Sdiff_iface`` (salinity diffusivity).

Also the non-local transport terms:

- ``kpp_Tnonlocal_iface`` (kpp_Tnonlocal_iface) and
- ``kpp_Snonlocal_iface`` (kpp_Snonlocal_iface).
