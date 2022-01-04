#################
Building the case
#################

This attempt intentionally tries to create a high-resolution compset on a
low-resolution grid.

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts/
   ./create_newcase --case /glade/p/work/johnsonb/cesm_runs/GIAF_JRA_HR.T62_g17.001 --compset GIAF_JRA_HR --res T62_g17 --mach cheyenne

.. error::

   .. code-block::

      ERROR: No description found for comp_class atm matching compsetname
      2000_DATM%JRA-1p4-2018-1p4-2018_SLND_CICE_POP2_DROF%JRA-1p4-2018_SGLC_SWAV
      in file /glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/config_component.xml,
      expected match in ['DATM'] % ['QIA', 'WISOQIA', 'CRU', 'CRUv7', 'GSWP3v1', 'CPLHIST', '1PT', 'NYF', 'IAF', 'JRA', 'JRA-1p4-2018']

This successfully creates a high-resolution compset on a low-resolution
grid using the GIAF compset.

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts/
   ./create_newcase --case /glade/work/johnsonb/cesm_runs/GIAF_HR.T62_g17.001 --compset GIAF_HR --res T62_g17 --mach cheyenne --run-unsupported --project P86850054
   Creating Case directory /glade/work/johnsonb/cesm_runs/GIAF_HR.T62_g17.001
