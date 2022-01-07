#########################
Processor element layouts
#########################

As described in the :doc:`building` page, the default processor element (PE) 
layout for an eighty-member CESM POP2 case on the ``f09_t13`` grid requires
242,880 total cores and there are only 145,152 cores on Cheyenne.

In order to run the benchmarking tests, the ensemble must be squeezed onto 
fewer PEs.

The default PE layout is:


.. code-block::

   cd /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_t13.001.e80
   ./xmlquery --partial PE
   [ ...  ]
   Results in group mach_pes
       NTASKS_PER_INST: ['ATM:72', 'LND:72', 'ICE:828', 'OCN:2208', 'ROF:72', 'GLC:72', 'WAV:72', 'ESP:1']
       ROOTPE: ['CPL:0', 'ATM:0', 'LND:0', 'ICE:0', 'OCN:828', 'ROF:0', 'GLC:0', 'WAV:0', 'ESP:0']

   Results in group mach_pes_last
       COSTPES_PER_NODE: 36
       COST_PES: 242892
       MAX_MPITASKS_PER_NODE: 36
       MAX_TASKS_PER_NODE: 36
       TOTALPES: 242880
   [ ...  ]


Two nodes (72 tasks) are set for each instance of:

- ATM, LND, ROF, GLC, and WAV

23 nodes (828 tasks) are set for each instance of:

- ICE

61 and 1/3 nodes are set for each instance of:

- OCN


Initial attempt
===============

For an initial ``f09_t13``  scale down, try the following nodes:

One node for each instance of:

- 1 node for ATM, LND, ROF, GLC, AND WAV

11 nodes for each instance of:

- ICE

30 nodes for each instance of:

- OCN


Working in ``DART_pop2-asd-project``
------------------------------------

The ``setup_CESM_hybrid_ensemble.csh`` script reads in ``DART_params.csh``.
Make ``t13`` versions of each of these scripts:

.. code-block::

   cp DART_params.csh DART_params_t13.csh
   cp setup_CESM_hybrid_ensemble.csh setup_CESM_hybrid_t13_ensemble.csh

