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


``config_compilers.xml``
========================

.. code-block:: xml

   <compiler MACH="derecho">
      <CFLAGS>
        <append> -march=core-avx2 </append>
      </CFLAGS>
      <FFLAGS>
        <append> -march=core-avx2 </append>
      </FFLAGS> 
      <!-- <SLIBS>
        <append> -qmkl </append>
      </SLIBS> -->
      <LDFLAGS>
         <append> -L$(NETCDF_PATH)/lib -lnetcdff -lnetcdf </append>
      </LDFLAGS>
   </compiler>

``config_machines.xml``
=======================

.. code-block:: xml

   <machine MACH="derecho">
      <DESC>NCAR AMD EPYC system</DESC>
      <NODENAME_REGEX>de.*.hpc.ucar.edu</NODENAME_REGEX>
      <OS>CNL</OS>
      <COMPILERS>intel,gnu,cray,nvhpc,intel-oneapi,intel-classic</COMPILERS>
      <MPILIBS>mpich</MPILIBS>
      <CIME_OUTPUT_ROOT>$ENV{SCRATCH}</CIME_OUTPUT_ROOT>
      <DIN_LOC_ROOT>$ENV{CESMDATAROOT}/inputdata</DIN_LOC_ROOT>
      <DIN_LOC_ROOT_CLMFORC>$ENV{CESMDATAROOT}/inputdata/atm/datm7</DIN_LOC_ROOT_CLMFORC>
      <DOUT_S_ROOT>$CIME_OUTPUT_ROOT/archive/$CASE</DOUT_S_ROOT>
      <BASELINE_ROOT>$ENV{CESMDATAROOT}/ccsm_baselines</BASELINE_ROOT>
      <CCSM_CPRNC>$ENV{CESMDATAROOT}/cprnc/cprnc</CCSM_CPRNC>
      <GMAKE_J>16</GMAKE_J>
      <BATCH_SYSTEM>pbs</BATCH_SYSTEM>
      <SUPPORTED_BY>cseg</SUPPORTED_BY>
      <MAX_TASKS_PER_NODE>128</MAX_TASKS_PER_NODE>
      <MAX_MPITASKS_PER_NODE>128</MAX_MPITASKS_PER_NODE>
      <PROJECT_REQUIRED>TRUE</PROJECT_REQUIRED>
      <mpirun mpilib="default">
         <executable>mpiexec</executable>
         <arguments>
            <arg name="label"> --label</arg>
            <arg name="buffer"> --line-buffer</arg>
            <arg name="num_tasks" > -n {{ total_tasks }}</arg>
         </arguments>
      </mpirun>
      <module_system type="module" allow_error="true">
         <!-- BKJ June 16, 2023 Trying to fix error: ERROR: module command $ENV{LMOD_ROOT}/lmod/libexec/lmod python load     cesmdev/1.0 ncarenv/23.06 -->
         <!-- <init_path lang="perl">$ENV{LMOD_ROOT}/lmod/init/perl</init_path>
         <init_path lang="python">$ENV{LMOD_ROOT}/lmod/init/env_modules_python.py</init_path>
         <init_path lang="sh">$ENV{LMOD_ROOT}/lmod/init/sh</init_path>
         <init_path lang="csh">$ENV{LMOD_ROOT}/lmod/init/csh</init_path>
         <cmd_path lang="perl">$ENV{LMOD_ROOT}/lmod/libexec/lmod perl</cmd_path>
         <cmd_path lang="python">$ENV{LMOD_ROOT}/lmod/libexec/lmod python</cmd_path> -->
         <init_path lang="perl">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/init/perl</init_path>
         <init_path lang="python">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/init/env_modules_python.py</init_path>
         <init_path lang="sh">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/init/sh</init_path>
         <init_path lang="csh">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/init/csh</init_path>
         <cmd_path lang="perl">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/libexec/lmod perl</cmd_path>
         <cmd_path lang="python">/glade/u/apps/derecho/23.06/spack/opt/spack/lmod/8.7.20/gcc/7.5.0/pdxb/lmod/lmod/libexec/lmod python</cmd_path>
         <cmd_path lang="sh">module</cmd_path>
         <cmd_path lang="csh">module</cmd_path>
         <modules>
         <!-- BKJ June 16, 2023 Trying to fix error: ERROR: module command $ENV{LMOD_ROOT}/lmod/libexec/lmod python load       cesmdev/1.0 ncarenv/23.06 -->
         <!-- <command name="load">cesmdev/1.0</command> -->
         <!-- <command name="load">ncarenv/23.06</command> -->
         <command name="purge"/>
         <command name="load">craype</command>
         </modules>
         <modules compiler="intel">
            <command name="load">intel/2023.0.0</command>
            <command name="load">mkl</command>
         </modules>
         <modules compiler="intel-oneapi">
            <command name="load">intel-oneapi/2023.0.0</command>
            <command name="load">mkl</command>
         </modules>
         <modules compiler="intel-classic">
            <command name="load">intel-classic/2023.0.0</command>
            <command name="load">mkl</command>
         </modules>
         <modules compiler="cray">
            <command name="load">cce/15.0.1</command>
            <command name="load">cray-libsci/23.02.1.1</command>
         </modules>
         <modules compiler="gnu">
            <command name="load">gcc/12.2.0</command>
            <command name="load">cray-libsci/23.02.1.1</command>
         </modules>
         <modules compiler="nvhpc">
            <command name="load">nvhpc/23.1</command>
         </modules>
         <modules>
            <command name="load">ncarcompilers/1.0.0</command>
            <command name="load">cmake</command>
         </modules>
         <modules mpilib="mpich">
            <command name="load">cray-mpich/8.1.25</command>
         </modules>
         <modules mpilib="mpi-serial">
            <command name="load">mpi-serial/2.3.0</command>
         </modules>
         <modules mpilib="mpi-serial">
            <command name="load">netcdf/4.9.2</command>
         </modules>
         <modules mpilib="!mpi-serial">
            <command name="load">netcdf-mpi/4.9.2</command>
            <command name="load">parallel-netcdf/1.12.3</command>
         </modules>
         <modules DEBUG="TRUE">
            <command name="load">parallelio/2.6.0-debug</command>
            <command name="load">esmf/8.5.0b23-debug</command>
         </modules>
         <modules DEBUG="FALSE">
            <command name="load">parallelio/2.6.0</command>
            <!-- BKJ June 16, 2023 -->
            <!-- <command name="load">esmf/8.5.0b23</command> -->
            <command name="load">esmf/8.4.2</command>
         </modules>
      </module_system>
      <environment_variables>
         <env name="OMP_STACKSIZE">64M</env>
         <env name="FI_CXI_RX_MATCH_MODE">hybrid</env>
         <!--      <env name="MPICH_COLL_SYNC">MPI_Bcast,MPI_AllReduce,MPI_Gatherv,MPI_Scatter,MPI_Scatterv,MPI_Reduce</   env> -->
         <env name="MPICH_COLL_SYNC">1</env>
         <env name="NETCDF_PATH">/glade/u/apps/derecho/23.06/spack/opt/spack/netcdf/4.9.2/intel-oneapi-mpi/2021.8.0/oneapi/2023.0.0/pbyg</env>
      </environment_variables>
      <!-- derecho has both gpfs and lustre file systems so I think this setting may cause issues -->
      <environment_variables mpilib="mpich">
         <env name="MPICH_MPIIO_HINTS">*:romio_cb_read=enable:romio_cb_write=enable:striping_factor=24</env>
      </environment_variables>
      <environment_variables comp_interface="nuopc">
         <!-- required on all systems for timing file output -->
         <env name="ESMF_RUNTIME_PROFILE">ON</env>
         <env name="ESMF_RUNTIME_PROFILE_OUTPUT">SUMMARY</env>
      </environment_variables>
   </machine>

``config_batch.xml``
====================

.. code-block:: xml

   <batch_system MACH="derecho" type="pbs" >
      <batch_submit>qsub</batch_submit>
      <directives>
         <directive default="/bin/bash" > -S {{ shell }}  </directive>
         <directive> -l select={{ num_nodes }}:ncpus={{ max_tasks_per_node }}:mpiprocs={{ tasks_per_node }}:ompthreads={{  thread_count }}</directive>
      </directives>
      <queues>
         <queue walltimemax="4:00:00" nodemin="1" nodemax="2488" >main</queue>
      </queues>
   </batch_system>
