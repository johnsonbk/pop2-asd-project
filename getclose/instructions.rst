#########################################
Creating runs for get_close caching tests
#########################################

Modified CESM installation
==========================

This project uses a custom grid within CESM that allows us to use the `CAM6 +
DART reanalysis <https://www.nature.com/articles/s41598-021-92927-0>`_ to
prescribe realistic temperature, precipitation/evaporation and momentum fluxes
from the atmosphere to the ocean. 

The reanalysis was computed on CAM's ``f09`` `grid <https://www.cesm.ucar.edu/models/cesm2/config/grids.html>`_.
POP's mesoscale eddy resolving high resolution grid, ``t13``, isn't used very
often because it is computationally intensive. So we defined a custom grid
configuration within CESM, ``f09_t13`` to couple CAM and POP together.

This custom grid definition is stored in this xml file in this CESM repository:

.. code-block::
   
   /glade/work/johnsonb/cesm2_1_1/cime/config/cesm/config_grids.xml

Repository containing setup scripts
===================================

The repository containing the setup scripts is here:

.. code-block::

   /glade/work/johnsonb/git/DART_pop2-asd-project

Only three of the scripts are actually used:

#. ``DART_params.csh``
#. ``copy_POP_JRA_restarts.py``
#. setup_CESM_startup_t13_ensemble.csh

``DART_params.csh``
-------------------

If you recursively copy the DART repository into the analagous subdirectory
in your work directory on glade, ``/glade/work/$USER/git/DART_pop2-asd-project``
you won't need to modify ``DART_params.csh``. Otherwise, you'll likely need to
change line 90:

.. code-block::

   90 setenv DARTROOT        /glade/work/${USER}/git/DART_pop2-asd-project

to match where y


Copy CESM SourceMods to your home directory
===========================================

In order to run POP with DART, specific source code files in POP's source code
need to be overwritten. This modified source code:

#. Inserts flags into CESM that enable data assimilation within the model
#. Modifies one of the physics routines -- the `barotripic mode solver <https://www.cesm.ucar.edu/models/cesm1.0/pop2/doc/users/node39.html>`_
   -- to ensure that DART's filter increments don't crash the solver

The setup script will stage the modified source code for you, but the modified
code must be present in your home directory in order to be staged properly.

.. code-block::

   cp -R cesm2_1_1 ~/

