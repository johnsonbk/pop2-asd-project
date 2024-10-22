##################
MOM6 in CESM 2.1.5
##################

MOM6 is the ocean component in CESM2.1.5. For detailed instructions about 
compiling a case with MOM6, consult the
`Github Wiki <https://github.com/ESCOMP/MOM_interface/wiki/Detailed-Instructions>`_.

.. code-block::

    git clone https://github.com/ESCOMP/CESM.git cesm2.1.5
    git switch release-cesm2.1.5
    ./bin/git-fleximod update
    ls components
        cam  cdeps  cice  cism  clm  cmeps  mizuroute  mom  mosart  rtm  ww3
    cd ../cime/scripts/
    ./create_newcase --res TL319_t232 --compset G_JRA --case /glade/work/johnsonb/cesm_runs/g.e215.G_JRA.TL319_t232.001 --mach derecho --run-unsupported --project PXXXXXXXX
    cd /glade/work/johnsonb/cesm_runs/g.e215.G_JRA.TL319_t232.001
    ./case.setup
    ./case.build
