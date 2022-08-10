#########################################
Creating runs for get_close caching tests
#########################################

Restart files for the initial ensemble
======================================

I've already copied the necessary POP and CICE files to a directory on scratch:

.. code-block::

   /glade/scratch/johnsonb/g.e20.G.TL319_t13.control.001/rest/0001-01-01-00000

If, at a later date, the files have been wiped from scratch, please let me know
and I'll rerun the script that copies them from campaign storage:

.. code-block::

   /glade/u/home/johnsonb/pyscripts/copy_restart_files.py

The restart files are from one of Alper Altuntas's high-resolution runs and
`there is restricted access to the directory on campaign storage <https://docs.dart.ucar.edu/en/latest/models/POP/readme.html#copy-pop-jra-restarts-py>`_
that they are stored in.

Modified CESM installation
==========================

This project uses a custom grid within CESM that allows us to use the `CAM6 +
DART reanalysis <https://www.nature.com/articles/s41598-021-92927-0>`_ to
prescribe realistic temperature, precipitation/evaporation and momentum fluxes
from the atmosphere to the ocean. 

The reanalysis was computed on CAM's ``f09`` `grid <https://www.cesm.ucar.edu/models/cesm2/config/grids.html>`_.
POP's mesoscale eddy resolving high resolution grid, ``t13``, isn't used very
often because integrations computed on it are computationally intensive. So I
defined a custom grid configuration within CESM, ``f09_t13`` to couple CAM and
POP together.

This custom grid definition is stored in this xml file in this CESM repository:

.. code-block::
   
   /glade/work/johnsonb/cesm2_1_1/cime/config/cesm/config_grids.xml

If you are running tests on Cheyenne, the setup scripts are already set to
reference this installation of CESM. If you are running the tests on another
system, this installation must be copied to that system.

Repository containing setup scripts
===================================

The relevant subdirectory of the repository containing the setup scripts is
here:

.. code-block::

   /glade/work/johnsonb/git/DART_pop2-asd-project/models/POP/shell_scripts/cesm2_1

If you recursively copy this DART repository into the analagous subdirectory
in your work directory on glade, ``/glade/work/$USER/git/DART_pop2-asd-project``
you won't need to modify the setup scripts.  

Only two of the scripts are actually used:

#. ``DART_params.csh``
#. ``setup_CESM_startup_t13_ensemble.csh``

``DART_params.csh``
-------------------

If you recursively copied the DART repository, the scripts should work without
modification. Otherwise, you'll likely need to change line 90 in
``DART_params.csh``:

.. code-block::

   90 setenv DARTROOT        /glade/work/${USER}/git/DART_pop2-asd-project

Ensure CESM SourceMods are present
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In order to run POP with DART, specific source code files in POP's source code
need to be overwritten. This modified source code:

#. Inserts flags into CESM that enable data assimilation within the model
#. Modifies one of the physics routines -- the `barotripic mode solver <https://www.cesm.ucar.edu/models/cesm1.0/pop2/doc/users/node39.html>`_
   -- to ensure that DART's filter increments don't crash the solver

The setup script will stage the modified source code for you. If it crashes,
ensure the SourceMods are present where ``DART_params.csh`` expects them:

.. code-block::

   export cesmtag=cesm2_1_1
   ls /glade/u/home/johnsonb/${cesmtag}/SourceMods

``setup_CESM_startup_t13_ensemble.csh``
=======================================

Setting up the case should be as simple as:

.. code-block::

   ./setup_CESM_startup_t13_ensemble.csh

.. important::

   POP is peculiar in that historically, the initial files were binary rather
   than netCDF.



