######################
Defining a custom grid
######################

In order to use the CAM6 reanalysis to force the ``t13`` ocean/sea ice grid, 
a ``f09_t13`` grid needs to be defined within CIME.

The `Model grids <https://esmci.github.io/cime/versions/master/html/users_guide/grids.html>`_
page in the CIME documentation provides instructions for doing so.


Available SCRIP grid files
==========================

CIME doesn't provide functionality to create Spherical Coordinate Remapping and
Interpolation Package (SCRIP) since it is a Los Alamos product `(Jones, 1999)
<https://doi.org/10.1175/1520-0493(1999)127\<2204:FASOCR\>2.0.CO;2>`_.

SCRIP files are the input files for the mapping utility, as described in Step 1
of `Adding grids <https://esmci.github.io/cime/versions/master/html/users_guide/grids.html#adding-grids>`_.
But, there should be SCRIP files for f09 and t13 already on GLADE since those
grids are already part of CESM. Let's start searching.

.. code-block::

   vi /glade/p/cesm/cseg/mapping/grids/README

   (2021-06-23)

   Old files here have been moved to the inputdata directory:

   inputdata/share/scripgrids
 
   Moving forward, all important scrip grid files (e.g., those that relate to
   standard CESM model resolutions) should be put in that directory and checked
   in to the inputdata repository. (ESMF mesh files should be put in
   inputdata/share/meshes.)

.. note::

   Hooray!

f09
---

The f09 scrip grids might be right here:

.. code-block::

   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_EW_SCRIP_desc.181018.nc
   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_NS_SCRIP_desc.181018.nc
   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_SCRIP_desc.181018.nc

t13
---

The t13 scrip grid might be right here:

.. code-block::

   /glade/p/cesm/cseg/inputdata/share/scripgrids/tx0.1v3_211102.nc

Generate mapping files
======================

Mapping files should be generated for:

.. code-block::

   atm <-> ocn
   atm <-> wav
   lnd <-> rof
   lnd <-> glc
   ocn <-> wav
   rof -> ocn

by calling ``gen_cesm_maps.sh`` in
``$CIMEROOT/tools/mapping/gen_mapping_files/``.

