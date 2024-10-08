###############################
Configuration of CIME XML files
###############################

.. note::

   A post from Michael Levy on the CESM2 slack channel: The CESM 2.1.5 release
   added derecho as a supported machine, is checking out that tag an option for
   you?

   .. code-block::

      git clone -b cesm2.1.5 https://github.com/ESCOMP/CESM.git cesm2_1_5
      cd cesm2_1_5
      ./manage_externals/checkout_externals
      cp /glade/work/johnsonb/cesm2_1_5/cime/config/cesm/machines/config_machines.xml /glade/work/johnsonb/cesm2_1_3/cime/config/cesm/machines

Use the configurations from cesm2.2.2:

.. code-block::

   cd /glade/work/johnsonb/cesm2_1_3/cime/config/cesm/machines
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/cesm/machines/config_batch.xml ./
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/cesm/machines/config_compilers.xml ./
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/cesm/machines/config_machines.xml ./
   
   cd /glade/work/johnsonb/cesm2_1_3/cime/config/xml_schemas/
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/xml_schemas/config_batch.xsd ./
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/xml_schemas/config_compilers_v2.xsd ./
   cp /glade/work/johnsonb/cesm2_2_2/cime/config/xml_schemas/config_machines.xsd ./

Test the compilation:

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/G.CAM6-Reanalysis.f09_g17.002 --compset GIAF --res f09_g17 --mach derecho --run-unsupported --project XXXXXXXX
