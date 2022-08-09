###############################
Generating the initial ensemble
###############################

For the benchmarking tests, the initial ensemble does not need the spread
necessary to prevent filter divergence.

Finding an initial set of restart files
=======================================
                                                                                 
Since HPSS is offline, it's unclear where the restart files are from
`Johnson et al. (2016) <https://doi.org/10.1175/JPO-D-15-0202.1>`_. But
(luckily) it's easier to find restart files on campaign storage than it was on
HPSS.

.. code-block::                                                                 

   sftp data-access.ucar.edu                                                    
   cd /gpfs/csfs1/cgd/oce                                                          
   find . -name "*t13*"

Copy files into an ensemble
===========================

Run a script to copy eighty sets of restart files into a restart directory on
scratch.

.. code-block::

   cd /glade/u/home/johnsonb/pyscripts
   python copy_restart_files.py
   ls -lart /glade/scratch/johnsonb/g.e20.G.TL319_t13.control.001/rest/0001-01-01-00000
   [ ... ]
   -rw-r--r-- 1 johnsonb ncar 11335683072 Jan  6 17:23 g.e20.G.TL319_t13.control.001.cice_0079.r.0001-01-01-00000.nc
   -rw-r--r-- 1 johnsonb ncar  8626923008 Jan  6 17:23 g.e20.G.TL319_t13.control.001.cpl_0079.r.0001-01-01-00000.nc
   -rw-r--r-- 1 johnsonb ncar 35251207680 Jan  6 17:23 g.e20.G.TL319_t13.control.001.pop_0079.r.0001-01-01-00000.nc
   -rw-r--r-- 1 johnsonb ncar 11335683072 Jan  6 17:23 g.e20.G.TL319_t13.control.001.cice_0080.r.0001-01-01-00000.nc
   -rw-r--r-- 1 johnsonb ncar  8626923008 Jan  6 17:23 g.e20.G.TL319_t13.control.001.cpl_0080.r.0001-01-01-00000.nc
   -rw-r--r-- 1 johnsonb ncar 35251207680 Jan  6 17:24 g.e20.G.TL319_t13.control.001.pop_0080.r.0001-01-01-00000.nc


t12 restart files
=================

These are actually from Frank's Yellowstone ASD experiment:

.. code-block::

   cp g.e12.G.T62_t12.003.pop.r.0006-02-01-00000.nc /glade/scratch/johnsonb/g.e12.G.T62_t12.003/rest/0006-02-01-00000/g.e12.G.T62_t12.003.pop_0001.r.0006-02-01-00000.nc
   cp g.e12.G.T62_t12.003.cice.r.0006-02-01-00000.nc /glade/scratch/johnsonb/g.e12.G.T62_t12.003/rest/0006-02-01-00000/g.e12.G.T62_t12.003.cice_0001.r.0006-02-01-00000.nc
   cp g.e12.G.T62_t12.003.cpl.r.0006-02-01-00000.nc /glade/scratch/johnsonb/g.e12.G.T62_t12.003/rest/0006-02-01-00000/g.e12.G.T62_t12.003.cpl_0001.r.0006-02-01-00000.nc

