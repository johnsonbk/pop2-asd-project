#####
CVMix
#####

The Community Vertical Mixing Project (CVMix) created a library of first-order
closures to parameterize vertical mixing in ocean models. CVMix is used in
POP2, MOM6 and MPAS ocean.

Installation instructions (for M2 MacBook Pro using MacPorts):

.. code-block::

   git clone https://github.com/CVMix/CVMix-src.git
   cd CVMix-src/src

Running make will invoke an executable, ``cvmix_setup``, that will configure the makefile.

.. code-block::

   make
   Fortran compiler (mpi not necessary): gfortran
   Directory containing netcdf configuration tool nc-config (or enter "no-nc-config" to    enter location of netcdf include and netcdf lib directories): /opt/local/bin
   [ messages from make ]

The resulting library file will be compiled here:

.. code-block::

   CVMix-src/lib/libcvmix.a

cvmix_data_type
===============

The ``cvmix_data_type`` is a derived type containing time-dependent variables
for each column in the domain. The type definition is defined in
``src/shared/cvmix_kinds_and_types.F90``.

It contains scalar variables such as integer-valued:

- ``nlev``

and real-valued:

- ``OceanDepth``
- ``BoundaryLayerDepth``
- ``lat``
- ``lon``
- ``Coriolis`` (Coriolis parameter), etc.

It contains many vector-valued variables corresponding to values at cell
centers or cell interfaces. Some examples are diffusivity coefficients at 
interfaces:

- ``Mdiff_iface`` (momentum diffusivity)
- ``Tdiff_iface`` (temperature diffusivity) and
- ``Sdiff_iface`` (salinity diffusivity).

Also the non-local transport terms:

- ``kpp_Tnonlocal_iface`` (kpp_Tnonlocal_iface) and
- ``kpp_Snonlocal_iface`` (kpp_Snonlocal_iface).

In ``vertical_mix.F90``, an array of ``cvmix_data_type`` is declared and
allocated:

.. code-block::

   type(cvmix_data_type), allocatable, dimension(:,:) :: CVmix_vars
   [ ... ]
   ncol = nx_block*ny_block
   allocate(CVmix_vars(ncol,nblocks_clinic))

So the dimensions of the array correspond to the number of columns in each
block and the number of blocks in the grid.

The ``init_vmix_kpp`` subroutine from ``vmix_kpp.F90`` gets called in
``init_vertical_mix`` and it uses the ``cvmix_put`` subroutine to set the
values in ``CVMix_vars``.

Augmenting CVmix to include additional fields
=============================================

Three files need to be edited in order to include additional fields in CVMix.

cvmix_kinds_and_types.F90
-------------------------

Since this module defines the ``cvmix_data_type`` derived type, additional 
arrays can be declared within the type definition. The first argument to the
mixing subroutines in POP2 is ``CVmix_vars`` which is an instance of the 
``cvmix_data_type``.

To add two new diffusivities for the diffusivity of temperature and salinity
via double diffusive convection, for example, declare two new arrays as
attributes within the type definition:

.. code-block::

   type, public :: cvmix_data_type
      [ ... ]
      real(cvmix_r8), dimension(:), pointer :: Tdiff_iface_ddiff => NULL()
      real(cvmix_r8), dimension(:), pointer :: Sdiff_iface_ddiff => NULL()
      ! units: m^2/s

cvmix_utils.F90
---------------

This module contains helper procedures that simplify other sections of the 
CVmix code.

.. note::

   Augmenting this module is optional, however, it makes the resulting code 
   more uniform with the preexisting code.

The ``cvmix_att_name`` function is an extensive case statement that maps
shortened attribute names to full length attribute names. Add new cases for the
newly declared attributes:

.. code-block::

   function cvmix_att_name(varname)
      character(len=*), intent(in) :: varname
      character(len=cvmix_strlen) :: cvmix_att_name

      select case(trim(varname))
         [ ... ]
         case ("Tdiff_ddiff")
            cvmix_att_name = "Tdiff_iface_ddiff"
         case ("Sdiff_ddiff")
            cvmix_att_name = "Sdiff_iface_ddiff"
         [ ... ]
      end select
   end function cvmix_att_name

cvmix_put_get.F90
-----------------

This module contains an interface to six subroutines used to assign values
to attributes of instances of the ``cvmix_data_type``. Since the new attributes 
declared above are real-valued vectors, augment the ``cvmix_put_real``
subroutine:

.. code-block::

   subroutine cvmix_put_real(CVmix_vars, varname, val, nlev_in)
      character(len=*),           intent(in) :: varname
      real(cvmix_r8),             intent(in) :: val
      integer,          optional, intent(in) :: nlev_in

      type(cvmix_data_type), intent(inout) :: CVmix_vars

      select case (trim(cvmix_att_name(varname)))
         [ ... ]

         case ("Tdiff_iface_ddiff")
            if (.not.associated(CVmix_vars%Tdiff_iface_ddiff)) then
              allocate(CVmix_vars%Tdiff_iface_ddiff(nlev+1))
            end if
            CVmix_vars%Tdiff_iface_ddiff(:) = val
         case ("Sdiff_iface_ddiff")
            if (.not.associated(CVmix_vars%Sdiff_iface_ddiff)) then
              allocate(CVmix_vars%Sdiff_iface_ddiff(nlev+1))
            end if
            CVmix_vars%Sdiff_iface_ddiff(:) = val
      
         [ ... ]
      end case
   end subroutine cvmix_put_real

   [ ... ]

and the ``cvmix_put_real_1D`` subroutine:

.. code-block::

   subroutine cvmix_put_real_1D(CVmix_vars, varname, val, nlev_in)
      character(len=*),             intent(in) :: varname
      real(cvmix_r8), dimension(:), intent(in) :: val
      integer,        optional,     intent(in) :: nlev_in

      type(cvmix_data_type), intent(inout) :: CVmix_vars
      
      select case (trim(cvmix_att_name(varname)))
         [ ... ]

         case ("Tdiff_iface_ddiff")
            if (.not.associated(CVmix_vars%Tdiff_iface_ddiff)) then
               allocate(CVmix_vars%Tdiff_iface_ddiff(nlev+1))
            end if
            CVmix_vars%Tdiff_iface_ddiff(:) = val
         case ("Sdiff_iface_ddiff")
            if (.not.associated(CVmix_vars%Sdiff_iface_ddiff)) then
               allocate(CVmix_vars%Sdiff_iface_ddiff(nlev+1))
            end if
            CVmix_vars%Sdiff_iface_ddiff(:) = val
         
         [ ... ]
      end case
   end subroutine cvmix_put_real_1D
