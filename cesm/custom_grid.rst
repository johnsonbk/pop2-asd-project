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

Using f05_t12 as a starting point
=================================

The most similar existing grid combination to the ``f09_t13`` grid that we are
attempting to create is the ``f05_t12`` grid. In 
``/glade/work/johnsonb/cesm2_1_3/cime/config/cesm/config_grids.xml``, there are
two relevant sections regarding ``f05_T12``:

.. code-block:: xml

   <!-- Line 375 -->
   <model_grid alias="f05_t12">
      <grid name="atm">0.47x0.63</grid>
      <grid name="lnd">0.47x0.63</grid>
      <grid name="ocnice">tx0.1v2</grid>
      <mask>tx0.1v2</mask>
   </model_grid>

   <!-- Line 1586 -->
   <gridmap atm_grid="0.47x0.63" ocn_grid="tx0.1v2">
      <map name="ATM2OCN_FMAPNAME">cpl/cpl6/map_fv0.47x0.63_to_tx0.1v2_aave_da_090218.nc</map>
      <map name="ATM2OCN_SMAPNAME">cpl/cpl6/map_fv0.47x0.63_to_tx0.1v2_bilin_da_090218.nc</map>
      <map name="ATM2OCN_VMAPNAME">cpl/cpl6/map_fv0.47x0.63_to_tx0.1v2_bilin_da_090218.nc</map>
      <map name="OCN2ATM_FMAPNAME">cpl/cpl6/map_tx0.1v2_to_fv0.47x0.63_aave_da_090218.nc</map>
      <map name="OCN2ATM_SMAPNAME">cpl/cpl6/map_tx0.1v2_to_fv0.47x0.63_aave_da_090218.nc</map>
   </gridmap>

It appears that we might *not* want patch mapping because the only ocean grids
that have patch mapping in ``config_grids.xml`` are these two ``gx1v6`` and
``gx3v7`` grid (out of scores of other grid configurations).

.. code-block:: xml

   <!-- Line 1578 -->
   <gridmap atm_grid="0.47x0.63" ocn_grid="gx1v6">
   <!-- Line 1833 -->
   <gridmap atm_grid="T31" ocn_grid="gx3v7">

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

First attempt
-------------

The initial step is to build the ``ESMF_RegridWeightGenCheck`` executable. 

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/check_maps/src
   gmake
   ls -lart ../
   [ ... ]
   drwxr-xr-x 3 johnsonb ncar    4096 Jan  3 16:09 .
   -rwxr-xr-x 1 johnsonb ncar 1624728 Jan  3 16:09 ESMF_RegridWeightGenCheck

So it appears the executable was compiled properly.

The next step is to run the ``gen_cesm_maps.sh`` script with the appropriate 
options:

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files
   ./gen_cesm_maps.sh --serial \
   --fileocn /glade/p/cesm/cseg/inputdata/share/scripgrids/tx0.1v3_211102.nc \
   --nameocn t13 \
   --fileatm /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_SCRIP_desc.181018.nc \
   --nameatm f09


