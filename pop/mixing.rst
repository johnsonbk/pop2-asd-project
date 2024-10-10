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

