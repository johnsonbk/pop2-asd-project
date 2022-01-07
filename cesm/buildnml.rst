########################
Modified buildnml script
########################

During the
`DART+CAM6 Reanalysis <https://doi.org/10.1038/s41598-021-92927-0>`_,
Kevin Raeder and Jeff Steward identified and fixed a bottleneck in one of the
MCT build scripts:

.. code-block::

   /glade/work/raeder/Models/cesm2_1_relsd_m5.6/cime/src/drivers/mct/cime_config/buildnml

The script runs slowly for a multi-instance CESM case because it attempts to
write multiple files simultaneously into directories that are locked.

This bottleneck is inevitable during the first submission of ``case.run``, but
it can be avoided on subsequent submissions.

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/src/drivers/mct/cime_config
   git mv buildnml buildnml.old
   cp /glade/work/raeder/Models/cesm2_1_relsd_m5.6/cime/src/drivers/mct/cime_config/buildnml ./

