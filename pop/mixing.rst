###########################
Modifications to POP Source
###########################

When ``vmix_coeffs`` is called, it is passed a block derived type (defined in
``blocks.F90``)

.. code-block::

   type, public :: block   ! block data type
      integer (int_kind) :: &
         block_id          ,&! global block number
         local_id          ,&! local address of block in current distrib
         ib, ie, jb, je    ,&! begin,end indices for physical domain
         iblock, jblock      ! cartesian i,j position for bloc

      integer (int_kind), dimension(:), pointer :: &
         i_glob, j_glob     ! global domain location for each point
   end type

So the subroutine is called once per block.

``VDC`` is a rank 5 array declared and allocated in ``vertical_mix.F90`` and
for KPP its fourth dimension has a length of 2 for salt and temperature.

.. code-block::

   allocate (VDC(nx_block,ny_block,0:km+1,2,nblocks_clinic)

``TRACER`` is a rank 6 array declared in ``prognostic.F90``:

.. code-block::

   real (r8), dimension(nx_block,ny_block,km,nt,3,max_blocks_clinic), &
      target :: &
      TRACER     ! 3d tracer fields for all blocks at 3 time levels
   
where ``nt`` is the number of tracers and is equal to ``2`` as declared in
``pop/test/unit/time_management/source/domain_size.F90``.

Namelist
========

The ``f09_g17`` vmix_kpp namelist setting from
``$CASEROOT/Buildconf/popconf/pop_in``:

.. code-block::

   &vmix_kpp_nml
     bckgrnd_vdc1 = 0.16
     bckgrnd_vdc2 = 0.0
     bckgrnd_vdc_ban = 1.0
     bckgrnd_vdc_dpth = 1000.0e02
     bckgrnd_vdc_eq = 0.01
     bckgrnd_vdc_linv = 4.5e-05
     bckgrnd_vdc_psim = 0.13
     langmuir_opt = 'vr12-ma'
     larctic_bckgrnd_vdc = .false.
     lcheckekmo = .false.
     lcvmix = .true.
     ldbl_diff = .true.
     lhoriz_varying_bckgrnd = .true.
     linertial = .false.
     lrich = .true.
     lshort_wave = .true.
     num_v_smooth_ri = 1
     prandtl = 10.0
     rich_mix = 50.0
   /

And these are the default parameters when the ``cvmix_init_ddiff`` subroutine
is called in ``vmix_kpp.F90``:

.. code-block::

   call cvmix_init_ddiff(strat_param_max=2.55_r8,                           &
                         kappa_ddiff_s=1e-4_r8,                             &
                         ddiff_exp1=1.0_r8,                                 &
                         ddiff_exp2=3.0_r8,                                 &
                         mol_diff=1.5e-6_r8,                                &
                         kappa_ddiff_param1=0.909_r8,                       &
                         kappa_ddiff_param2=4.6_r8,                         &
                         kappa_ddiff_param3=-0.54_r8,                       &
                         diff_conv_type="MC76")

.. note::

   ``old_vals`` aren't specified as a keyword argument.
