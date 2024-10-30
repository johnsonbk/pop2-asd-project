#######################################
Saving an additional time average field
#######################################

.. note::

   This documentation already exists in the
   `POP2 documentation <https://www2.cesm.ucar.edu/models/cesm1.2/pop2/doc/faq.njn/#dev_tavg_add3>`_
   but it's being duplicated here in case the original website is taken down.

First, search the CESM POP2 Fortran modules to see if your variable, ``VARNAME``, is a model
variable. If the model variable exists, but the tavg "handle" (``tavg_VARNAME``) associated with the
model variable does not, you will need to add new code to the POP2 model.

Suppose you are interested in putting the time average of the U velocity at level five into a POP2
time-averaged output file. You've already searched the gx1v6_tavg_contents file, but U velocity at
level five is not on the list. Next, you've searched the POP2 modules, and although you found
references to UVEL, tavg_UVEL, tavg_U1_8, and lots of tavg_U* variables, you've found nothing that
looks like U velocity at level five.

Because under this scenario you have not found what you want, you will need to modify the POP2 code.
Note that in this example, you could also just extract level five from the ``3D UVEL`` that is
already written out to the ``$CASE.pop.h.yyyy*`` file, but let's just go with this example for now,
assuming that you have good reason to want just U velocity at level five as a separate output
variable.

In adding support for new tavg output variables, it is easiest to mimic developments for similar
variables, so let's use ``tavg_U1_8``, the vertical average of ``U`` velocity over model levels one
through eight, as our template. To instrument the POP2 code to write out our new varible, U
velocity at level five, do the following:

.. code-block::

   $cd $CASE/SourceMods/src.pop2
   cp $CODEROOT/models/ocn/pop2/source/baroclinic.F90

Edit baroclinic, using ``tavg_U1_8`` as a guide, adding the following lines in the appropriate
places:

.. code-block::

   integer (int_kind) :: tavg_U5_5  ! U velocity in layer five


   call define_tavg_field(tavg_U5_5,'U5_5',2,                      &
                          long_name='Zonal Velocity lvls 5-5',     &
                          units='centimeter/s', grid_loc='2221')
   
   if (k == 5)  &
      call accumulate_tavg_field(UVEL(:,:,k,curtime,iblock),tavg_U5_5,iblock,k)

After making these modifications:

.. code-block::

   cd $CASE
   ./case.build
   
Then run your ``$CASE``, examine your ``$CASE.pop.h.yyyy-mm.nc`` file, where you should find your
new variable, ``U5_5``.
