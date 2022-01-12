################
List of attempts
################

CESM 2.1.0
==========

CESM 2.1.1
==========

GIAF_HR.cesm2_1_1.T62_t13.001
-----------------------------

Build
~~~~~

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.T62_t13.001 --compset GIAF_HR --res T62_t13 --mach cheyenne --run-unsupported --project P86850054
   Compset longname is 2000_DATM%IAF_SLND_CICE_POP2_DROF%IAF_SGLC_SWAV
   cd /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.T62_t13.001
   ./xmlquery STOP_N
	   STOP_N: 3

Result
~~~~~~

Execution terminated
Exit_status=0
resources_used.cpupercent=81
resources_used.cput=00:00:14
resources_used.mem=34468kb
resources_used.ncpus=1
resources_used.vmem=64996kb
resources_used.walltime=00:00:15

.. note::

   Perplexingly, this runs to "completion" but a POP2 restart file is not
   produced.

GIAF_HR.cesm2_1_1.T62_t12.001
-----------------------------

Build
~~~~~

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.T62_t12.001 --compset GIAF_HR --res T62_t12 --mach cheyenne --run-unsupported --project P86850054
   Compset longname is 2000_DATM%IAF_SLND_CICE_POP2_DROF%IAF_SGLC_SWAV
   cd /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.T62_t12.001
   ./xmlquery STOP_N
	   STOP_N: 3 

Result
~~~~~~

Execution terminated
Exit_status=1
resources_used.cpupercent=98002
resources_used.cput=91:35:53
resources_used.mem=679832624kb
resources_used.ncpus=1044
resources_used.vmem=5449231040kb
resources_used.walltime=00:06:06

CESM 2.1.3
==========
