##########
User Guide
##########

Logging on to the system
========================

.. code-block::

   ssh $USER@derecho.hpc.ucar.edu

Queues
======

Ben Kirk posted to Slack to use the following command to test queue access:

.. code-block::

   qcmd -l select=2:ncpus=128:mpiprocs=128:mem=200G -A A####### -l walltime=0:05:00 -- "date && hostname"
   Waiting on job launch; 1230726.desched1 with qsub arguments:
      qsub  -l select=2:ncpus=128:mpiprocs=128:mem=200G -A A####### -q main@desched1 -l walltime=0:05:00

   Thu 15 Jun 2023 10:45:21 PM MDT
   dec0476

This command gives an overview of the queues:

.. code-block::

   qstat -Qf

MPI Job Example
===============

.. code-block:: bash

   #!/bin/bash
   #PBS -A project_code
   #PBS -N mpi_job
   #PBS -q main 
   #PBS -l walltime=01:00:00 
   #PBS -l select=2:ncpus=128:mpiprocs=128
   
   # load necessary module environment
   module purge
   module load ncarenv craype cce cray-mpich
   
   # Run application using cray-mpich MPI
   mpiexec -n 256 -ppn 128 ./executable_name

