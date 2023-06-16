##########
User Guide
##########

Logging on to the system
========================

.. code-block::

   ssh $USER@derecho.hpc.ucar.edu

Porting CESM
============

A shortcut to finding a suitable configuration for porting CESM to a machine is
to copy a port to a similarly configured machine.

`Derecho <https://arc.ucar.edu/knowledge_base/74317833>`_ is a Cray EX cluster,
just like `Perlmutter <https://docs.nersc.gov/systems/perlmutter/architecture/>`_.
Both machines have:

- 2x AMD EPYC 7763 (Milan) CPUs per node
- 64 cores per CPU
- AVX2 instruction set

Note that Perlmutter has 512 GB of DDR4 memory while Derecho only has 256 GB 
DDR4 memory per node.

I've checked out ``release-cesm2.2.1`` and it doesn't seem as if there is a 
Perlmutter entry in the ``config_machines.xml`` file.

Jim Edwards has been posting the path to a sandbox CESM repository in his home
directory:

.. code-block::

   /glade/u/home/jedwards/sandboxes/cesm2_x_alpha/ccs_config/machines/config_machines.xml

It has configurations for Perlmutter, Gust and Derecho, so I've copied it to my
home directory:

.. code-block::

   cp ~/jedwards_sandbox/config_machines.xml

config_machines.xml
===================

I added the Derecho entry to this ``config_machines.xml`` file:

.. code-block::

   /glade/work/johnsonb/cesm2_1_3/cime/config/cesm/machines/config_machines.xml

This cesm repository also has the f09_t13 grid properly set up.


