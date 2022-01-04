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

f09
---

The f09 scrip grids files are here:

.. code-block::

   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_EW_SCRIP_desc.181018.nc
   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_NS_SCRIP_desc.181018.nc
   /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_SCRIP_desc.181018.nc

t13
---

The t13 scrip grid files are here:

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

Interesting note: no domain file
--------------------------------

There is no domain entry defined for the ``f05_t12`` grid:

.. code-block:: xml

   <!-- Line 1119 -->
   <domain name="0.47x0.63">
      <nx>576</nx>  <ny>384</ny>
      <file grid="atm|lnd" mask="gx1v6">domain.lnd.fv0.47x0.63_gx1v6.090407.nc</file>
      <file grid="ocnice"  mask="gx1v6">domain.ocn.0.47x0.63_gx1v6_090408.nc</file>
      <file grid="atm|lnd" mask="gx1v7">domain.lnd.fv0.47x0.63_gx1v7.180521.nc</file>
      <file grid="ocnice"  mask="gx1v7">domain.ocn.fv0.47x0.63_gx1v7.180521.nc</file>
      <desc>0.47x0.63 is FV 1/2-deg grid:</desc>
   </domain>

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

Build the executable
--------------------

The initial step is to build the ``ESMF_RegridWeightGenCheck`` executable. 

.. code-block::
   
   module load esmf_libs/8.0.0
   module load esmf-8.0.0-ncdfio-uni-O
   gmake
   ls -lart ../
   [ ... ]
   drwxr-xr-x 3 johnsonb ncar    4096 Jan  4 06:58 .
   -rwxr-xr-x 1 johnsonb ncar 1624728 Jan  4 06:58 ESMF_RegridWeightGenCheck

Run the script
--------------

The next step is to run the ``gen_cesm_maps.sh`` script with the appropriate 
options:

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files
   qsub -I -l select=1:ncpus=36:mpiprocs=36 -l walltime=01:00:00 -q regular -A P86850054
   ./gen_cesm_maps.sh \
   --fileocn /glade/p/cesm/cseg/inputdata/share/scripgrids/tx0.1v3_211102.nc \
   --nameocn tx0.1v3 \
   --fileatm /glade/p/cesm/cseg/inputdata/share/scripgrids/0.9x1.25_SCRIP_desc.181018.nc \
   --nameatm fv0.9x1.25
   [ ...  ]
   Tue Jan  4 13:34:36 MST 2022
   1: map_tx0.1v3_TO_fv0.9x1.25_aave.220104.nc
   All           21  tests passed!
   -----
   2: map_tx0.1v3_TO_fv0.9x1.25_blin.220104.nc
   All           14  tests passed!
   -----
   3: map_fv0.9x1.25_TO_tx0.1v3_aave.220104.nc
   All           21  tests passed!
   -----
   4: map_fv0.9x1.25_TO_tx0.1v3_blin.220104.nc
   All           14  tests passed!
   -----
   5: map_fv0.9x1.25_TO_tx0.1v3_patc.220104.nc
   All           14  tests passed!
   -----

So it appears the mapping files were created properly.

Resultant files
---------------

The script creates two sets of files in 
``/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files``

Mapping ocean to atmosphere
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Conservative, area-averaged: ``map_tx0.1v3_TO_fv0.9x1.25_aave.220104.nc``
- Non-conservative, bilinear: ``map_tx0.1v3_TO_fv0.9x1.25_blin.220104.nc``

Mapping atmosphere to ocean
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Conservative, area-averaged: ``map_fv0.9x1.25_TO_tx0.1v3_aave.220104.nc``
- Non-conservative, bilinear: ``map_fv0.9x1.25_TO_tx0.1v3_blin.220104.nc``
- Non-conservative, patch: ``map_fv0.9x1.25_TO_tx0.1v3_patc.220104.nc``

.. caution::

   Before the actual experiment begins, check to see if a new ``rtm->ocn``
   mapping file must be created. The
   `Adding grids <https://esmci.github.io/cime/versions/master/html/users_guide/grids.html#adding-grids>`_
   section of the CIME documentation says that the, "process for mapping from
   the runoff grid to the ocean grid is currently undergoing many
   changes...please contact Michael Levy for assistance." 

Generate domain files
=====================

Build the executable
--------------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_domain_files
   more INSTALL
   # Read the installation instructions
   cd src
   ../../../configure --macros-format Makefile --mpilib mpi-serial
   (. ./.env_mach_specific.sh ; gmake)
   ls -lart ../
   [ ...  ]
   drwxr-xr-x 3 johnsonb ncar    4096 Jan  4 14:52 .
   -rwxr-xr-x 1 johnsonb ncar 6086848 Jan  4 14:52 gen_domain

Pass the executable the correct files
-------------------------------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_domain_files
   ./gen_domain -m ../gen_mapping_files/map_tx0.1v3_TO_fv0.9x1.25_aave.220104.nc -o tx0.1v3 -l fv0.9x1.25
   [ ... ]
   write domain.lnd.fv0.9x1.25_tx0.1v3.220104.nc
   successfully created domain file domain.lnd.fv0.9x1.25_tx0.1v3.220104.nc
   write domain.ocn.fv0.9x1.25_tx0.1v3.220104.nc
   successfully created domain file domain.ocn.fv0.9x1.25_tx0.1v3.220104.nc

Edit config_grids.xml
=====================

Next edit config_grids.xml to add a ``model_grid`` node, a ``gridmap`` node and
possibly a ``domain`` entry for every new component grid.

Define the model grid
---------------------

.. code-block:: xml

   <!-- New f09_t13 grid for DART+POP2 ASD Project -->
   <model_grid alias="f09_t13">
      <grid name="atm">0.9x1.25</grid>
      <grid name="lnd">0.9x1.25</grid>
      <grid name="ocnice">tx0.1v3</grid>
      <mask>tx0.1v3</mask>
   </model_grid>

Add the domain files
--------------------

.. code-block:: xml

   <!-- Line 1128 -->
   <domain name="0.9x1.25">
      <nx>288</nx>  <ny>192</ny>
      <file grid="atm|lnd" mask="gx1v6">domain.lnd.fv0.9x1.25_gx1v6.090309.nc</file>
      <file grid="ocnice"  mask="gx1v6">domain.ocn.0.9x1.25_gx1v6_090403.nc</file>
      <file grid="atm|lnd" mask="gx1v7">domain.lnd.fv0.9x1.25_gx1v7.151020.nc</file>
      <file grid="ocnice"  mask="gx1v7">domain.ocn.fv0.9x1.25_gx1v7.151020.nc</file>
      <file grid="atm|lnd" mask="tn1v3">domain.lnd.fv0.9x1.25_tn1v3.160414.nc</file>
      <file grid="ocnice"  mask="tn1v3">domain.ocn.fv0.9x1.25_tn1v3.160414.nc</file>
      <file grid="atm|lnd" mask="tn0.25v3">domain.lnd.fv0.9x1.25_tn0.25v3.160721.nc</file>
      <file grid="ocnice"  mask="tn0.25v3">domain.ocn.fv0.9x1.25_tn0.25v3.160721.nc</file>
      <file grid="atm|lnd" mask="null">/glade/u/home/benedict/ys/datain/domain.aqua.fv0.9x1.25.nc</file>
      <file grid="ocnice"  mask="null">/glade/u/home/benedict/ys/datain/domain.aqua.fv0.9x1.25.nc</file>
      <!-- New f09_t13 domains for DART+POP2 ASD Project -->
      <file grid="atm|lnd" mask="tx0.1v3">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_domain_files/domain.lnd.fv0.9x1.25_tx0.1v3.220104.nc</file>
      <file grid="ocnice"  mask="tx0.1v3">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_domain_files/domain.ocn.fv0.9x1.25_tx0.1v3.220104.nc</file>
      <desc>0.9x1.25 is FV 1-deg grid:</desc>
   </domain>

Declare the grid maps
---------------------

.. code-block:: xml

   <!-- New f09_t13 gridmap for DART+POP2 ASD Project -->
   <gridmap atm_grid="0.9x1.25" ocn_grid="tx0.1v3">
      <map name="ATM2OCN_FMAPNAME">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files/map_fv0.9x1.25_TO_tx0.1v3_aave.220104.nc</map>
      <map name="ATM2OCN_SMAPNAME">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files/map_fv0.9x1.25_TO_tx0.1v3_blin.220104.nc</map>
      <map name="ATM2OCN_VMAPNAME">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files/map_fv0.9x1.25_TO_tx0.1v3_blin.220104.nc</map>
      <map name="OCN2ATM_FMAPNAME">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files/map_tx0.1v3_TO_fv0.9x1.25_aave.220104.nc</map>
      <map name="OCN2ATM_SMAPNAME">/glade/work/johnsonb/cesm2_1_3/cime/tools/mapping/gen_mapping_files/map_tx0.1v3_TO_fv0.9x1.25_aave.220104.nc</map>
   </gridmap>

Check the XML file
------------------

Check the formatting of the XML file to ensure it is formatted properly:

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/config/cesm
   xmllint --noout --schema /glade/work/johnsonb/cesm2_1_3/cime/config/xml_schemas/config_grids_v2.xsd ./config_grids.xml
   ./config_grids.xml validates

.. note::

   Hooray!
