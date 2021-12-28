#####
Grids
#####

The project compares the behavior of POP2 ensembles using two different grid
configurations: a low-resolution, eddy-parameterizing grid and a
high-resolution, eddy-resolving grid. 

The default available atmospheric grids for each are listed on the
`CESM Grid Resolution Definitions
<https://www.cesm.ucar.edu/models/cesm2/config/grids.html>`_ and are reproduced
below.

Low-resolution, eddy parameterizing
===================================

The CESM grid used for the Coupled Model Intercomparison Project (CMIP) is a
1° grid with a displaced Greenland pole. It has two configurations within CESM:
``g16`` and ``g17``.

This is the same ocean/ice grid used for the  `CESM large ensemble
<https://journals.ametsoc.org/view/journals/bams/96/8/bams-d-13-00255.1.xml>`_
and the `DART CAM6 Reanalysis
<https://www.nature.com/articles/s41598-021-92927-0>`_.

Each of these grids has been coupled to different atmospheric and land grids, 
which are listed in the *Default configurations* table rows below.

+----------------+-----------------------------------------------------------------------------+
| Grid           | g16                                                                         |
+================+=============================================================================+
| Grid type      | gx1v6 is displaced Greenland pole v6 1-deg grid                             |
+----------------+-----------------------------------------------------------------------------+
| Domain file    | ``/glade/p/cesm/cseg/inputdata/share/domains/domain.ocn.gx1v6.090206.nc``   |
+----------------+-----------------------------------------------------------------------------+
| Domain         | |g16|                                                                       |
+----------------+-----------------------------------------------------------------------------+
| Default        | f02_g16, f05_g16, f09_g16, f19_g16, ne120_g16, ne240_f02_g16, ne30_f09_g16, |
| configurations | ne30_f19_g16, ne30_g16, ne60_g16, T62_g16                                   |
+----------------+-----------------------------------------------------------------------------+

+----------------+-----------------------------------------------------------------------------+
| Grid           | g17                                                                         |
+================+=============================================================================+
| Grid type      | gx1v7 is displaced Greenland pole 1-deg grid with Caspian as a land feature |
+----------------+-----------------------------------------------------------------------------+
| Domain file    | ``/glade/p/cesm/cseg/inputdata/share/domains/domain.ocn.gx1v7.151008.nc``   |
+----------------+-----------------------------------------------------------------------------+
| Domain         | |g17|                                                                       |
+----------------+-----------------------------------------------------------------------------+
| Default        | f02_g17, f05_g17, f09_g17, f19_g17, f19_g17_r01, f19_g17_r05, g17_g17,      |
| configurations | ne0CONUSne30x8_g17,  ne120_g17, ne120pg3_g17, ne16_g17, ne240_f02_g17,      |
|                | ne30_f09_g17, ne30_f19_g17, ne30_g17, ne30pg3_g17, ne60_g17, T62_g17,       |
|                | TL319_g17, TL639_g17                                                        |
+----------------+-----------------------------------------------------------------------------+

High-resolution, eddy resolving
===============================

The ~0.1° ocean/sea ice grid has two different configuratons within CESM:
``t12`` and ``t13``. Each of these grids has been coupled to different
atmospheric and land grids. This is the same ocean/ice grid used in `Johnson
et al. (2016) <https://doi.org/10.1175/JPO-D-15-0202.1>`_ and
`Deppenmeier et al. (2021) <https://doi.org/10.1175/JPO-D-20-0217.1>`_.

Each of these grids has been coupled to different atmospheric and land grids, 
which are listed in the *Default configurations* table rows below.

+----------------+-----------------------------------------------------------------------------+
| Grid           | t12                                                                         |
+================+=============================================================================+
| Grid type      | tx0.1v2 is tripole v2 1/10-deg grid                                         |
+----------------+-----------------------------------------------------------------------------+
| Domain file    | ``/glade/p/cesm/cseg/inputdata/share/domains/domain.ocn.tx0.1v2.161014.nc`` |
+----------------+-----------------------------------------------------------------------------+
| Domain         | |t12|                                                                       |
+----------------+-----------------------------------------------------------------------------+
| Default        | f02_t12, f05_t12, ne120_t12, ne240_t12, T341_f02_t12, T62_t12               |
| configurations |                                                                             |
+----------------+-----------------------------------------------------------------------------+

+----------------+-----------------------------------------------------------------------------+
| Grid           | t13                                                                         |
+================+=============================================================================+
| Grid type      | tx0.1v3 is tripole v3 1/10-deg grid with Caspian as a land feature          |
+----------------+-----------------------------------------------------------------------------+
| Domain file    | ``/glade/p/cesm/cseg/inputdata/share/domains/domain.ocn.tx0.1v3.170730.nc`` |
+----------------+-----------------------------------------------------------------------------+
| Domain         | |t13|                                                                       |
+----------------+-----------------------------------------------------------------------------+
| Default        | ne120pg3_t13, T62_t13, TL319_t13                                            |
| Configurations |                                                                             |
+----------------+-----------------------------------------------------------------------------+


.. |t12| image:: /_static/t12.png
   :width: 525px

.. |t13| image:: /_static/t13.png
   :width: 525px

.. |g16| image:: /_static/g16.png
   :width: 525px

.. |g17| image:: /_static/g17.png
   :width: 525px

