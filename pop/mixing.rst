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
