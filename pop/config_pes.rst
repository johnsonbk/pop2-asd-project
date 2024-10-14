##########
Config PEs
##########

The processor element (PE) layouts are stored in a subdirectory for each 
component. For POP, they are stored here:

.. code-block::

   $CESMROOT/components/pop/cime_config/config_pes.xml

These can be modified for each grid resolution.

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/components/pop/cime_config
   cp /glade/work/johnsonb/cesm2_1_5/components/pop/cime_config/config_pes.xml ./
