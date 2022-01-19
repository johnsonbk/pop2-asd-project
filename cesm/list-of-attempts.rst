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

.. error::

   Warning: Departure points out of bounds in remap
   [ ...  ]
   remap transport: bad departure points
   ERROR: remap transport: bad departure points

Chris Riedel linked to the relevant page in the
`CICE documentation <https://cesmcice.readthedocs.io/en/latest/users_guide/ice_troubleshoot.html>`_
for this problem. Editing ``user_nl_cice`` fixes the problem.

.. code-block::

   cd $CASEROOT
   vim user_nl_cice_00*
   ! Append this line to the end of the file
   ndtd = 5

G.benchmarking.cesm2_1_1.f09_t12.001
------------------------------------

Build
~~~~~

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.benchmarking.cesm2_1_1.f09_t12.001 --compset GIAF_HR --res f09_t12 --mach cheyenne --run-unsupported --project P86850054
   cd /glade/work/johnsonb/cesm_runs/G.benchmarking.cesm2_1_1.f09_t12.001
   ./case.setup
   qcmd -q share -l select=1 -A P86850054 -- ./case.build
   [ ... ]
   MODEL BUILD HAS FINISHED SUCCESSFULLY
   ./xmlchange STOP_N=3
   ./case.submit -M begin,end

Result
~~~~~~

Execution terminated
Exit_status=1
resources_used.cpupercent=95809
resources_used.cput=84:07:17
resources_used.mem=676683064kb
resources_used.ncpus=1044
resources_used.vmem=5449458968kb
resources_used.walltime=00:06:18

.. error::

    Warning: Departure points out of bounds in remap
    [ ...  ]
    remap transport: bad departure points
    ERROR: remap transport: bad departure points

.. note::

   It's promising that the ``T62_t12`` case and the ``f09_t12`` both encounter
   the CFL-like error.

GIAF_HR.cesm2_1_1.f09_t13.002
-----------------------------

.. important::

   Since the G.benchmarking.cesm2_1_1.f09_t12.001 run hit the CFL-like error, 
   it suggests that the problem with the f09_t13 setup has to do with the
   mapping and domain files. This GIAF_HR.cesm2_1_1.f09_t13.002 case rebuilds
   the mapping and domain files with a different SCRIP file:
   /glade/p/cesm/cseg/inputdata/share/scripgrids/tx0.1v3_170728.nc

Build
~~~~~

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.f09_t13.002 --compset GIAF_HR --res f09_t13 --mach cheyenne --run-unsupported --project P86850054
   cd /glade/work/johnsonb/cesm_runs/GIAF_HR.cesm2_1_1.f09_t13.002
   ./case.setup
   qcmd -q share -l select=1 -A P86850054 -- ./case.build
   [ ... ]
   MODEL BUILD HAS FINISHED SUCCESSFULLY
   ./xmlchange STOP_N=3
   ./case.submit -M begin,end

Result
~~~~~~

Execution terminated
Exit_status=0
resources_used.cpupercent=295569
resources_used.cput=364:38:03
resources_used.mem=1158844040kb
resources_used.ncpus=3060
resources_used.vmem=7207103764kb
resources_used.walltime=00:08:27

.. note::

   This run ran to completion and output a POP restart file. This suggests that
   the SCRIP file was the issue confounding earlier t13 runs.

CESM 2.1.3
==========
