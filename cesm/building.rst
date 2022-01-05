#################
Building the case
#################

f09_g17
=======

Single instance
---------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts
   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_g17.001 --compset GIAF --res f09_g17 --mach cheyenne --run-unsupported --project P86850054
   cd /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_g17.001
   ./case.setup
   ./xmlquery --partial PE
   [ ... ]
   Results in group mach_pes
       NTASKS_PER_INST: ['ATM:72', 'LND:72', 'ICE:72', 'OCN:144', 'ROF:72', 'GLC:72', 'WAV:72', 'ESP:1']
       ROOTPE: ['CPL:0', 'ATM:0', 'LND:0', 'ICE:0', 'OCN:72', 'ROF:0', 'GLC:0', 'WAV:0', 'ESP:0']

   Results in group mach_pes_last
       COSTPES_PER_NODE: 36
       COST_PES: 216
       MAX_MPITASKS_PER_NODE: 36
       MAX_TASKS_PER_NODE: 36
       TOTALPES: 216
   [ ... ]

Eighty members
--------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts
   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_g17.001.e80 --compset GIAF --res f09_g17 --mach cheyenne --run-unsupported --project P86850054 --ninst 80 --multi-driver
   cd /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_g17.001.e80
   ./case.setup
   ./xmlquery --partial PE
   [ ... ]
   Results in group mach_pes
	   NTASKS_PER_INST: ['ATM:72', 'LND:72', 'ICE:72', 'OCN:144', 'ROF:72', 'GLC:72', 'WAV:72', 'ESP:1']
	   ROOTPE: ['CPL:0', 'ATM:0', 'LND:0', 'ICE:0', 'OCN:72', 'ROF:0', 'GLC:0', 'WAV:0', 'ESP:0']

   Results in group mach_pes_last
	   COSTPES_PER_NODE: 36
	   COST_PES: 17280
	   MAX_MPITASKS_PER_NODE: 36
	   MAX_TASKS_PER_NODE: 36
	   TOTALPES: 17280
   [ ... ]

f09_t13
=======

Use the ``GIAF_HR`` compset as a starting point for ``f09_t13``.

Single instance
---------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts
   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_t13.001 --compset GIAF_HR --res f09_t13 --mach cheyenne --run-unsupported --project P86850054
   cd /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_t13.001
   ./case.setup
   ./xmlquery --partial PE
   [ ... ]
   Results in group mach_pes
       NTASKS_PER_INST: ['ATM:72', 'LND:72', 'ICE:828', 'OCN:2208', 'ROF:72', 'GLC:72', 'WAV:72', 'ESP:1']
       ROOTPE: ['CPL:0', 'ATM:0', 'LND:0', 'ICE:0', 'OCN:828', 'ROF:0', 'GLC:0', 'WAV:0', 'ESP:0']
   
   Results in group mach_pes_last
       COSTPES_PER_NODE: 36
       COST_PES: 3060
       MAX_MPITASKS_PER_NODE: 36
       MAX_TASKS_PER_NODE: 36
       TOTALPES: 3036
   [ ... ]

Eighty members
--------------

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/scripts
   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_t13.001.e80 --compset GIAF_HR --res f09_t13 --mach cheyenne --run-unsupported --project P86850054 --ninst 80 --multi-driver
   cd /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_t13.001.e80
   ./case.setup
   ./xmlquery --partial PE
   [ ... ]
   Results in group mach_pes
	   NTASKS_PER_INST: ['ATM:72', 'LND:72', 'ICE:828', 'OCN:2208', 'ROF:72', 'GLC:72', 'WAV:72', 'ESP:1']
	   ROOTPE: ['CPL:0', 'ATM:0', 'LND:0', 'ICE:0', 'OCN:828', 'ROF:0', 'GLC:0', 'WAV:0', 'ESP:0']

   Results in group mach_pes_last
	   COSTPES_PER_NODE: 36
	   COST_PES: 242892
	   MAX_MPITASKS_PER_NODE: 36
	   MAX_TASKS_PER_NODE: 36
	   TOTALPES: 242880
