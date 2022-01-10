########################
Build error in CESM2.1.3
########################

.. error::

   A build error occurs when trying to build with CESM2.1.3. This error doesn't
   occur with CESM2.1.0.

.. code-block::

   Building case in directory /glade/work/johnsonb/cases/G.benchmarking.f09_t13_e5
   sharedlib_only is False
   model_only is False
   Setting resource.RLIMIT_STACK to -1 from (-1, -1)
   Generating component namelists as part of build
   GET_REFCASE is false, the user is expected to stage the refcase to the run directory.
   Creating component namelists
      Calling /glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml
      Running /glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml
  Traceback (most recent call last):
     File "/glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml", line 251, in <module>
       _main_func()
     File "/glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml", line 247, in _main_func
       buildnml(case, caseroot, "datm")
     File "/glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml", line 229, in buildnml
       _create_namelists(case, confdir, inst_string, namelist_infile, nmlgen, data_list_path)
     File "/glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml", line 123, in _create_namelists
       nmlgen.create_stream_file_and_update_shr_strdata_nml(config, case.get_value("CASEROOT"),stream, stream_path, data_list_path)
     File "/glade/work/johnsonb/cesm2_1_3/cime/scripts/Tools/../../scripts/lib/CIME/nmlgen.py", line 444, in create_stream_file_and_update_shr_strdata_nml
       strmobj = Stream(infile=stream_path)
     File "/glade/work/johnsonb/cesm2_1_3/cime/scripts/Tools/../../scripts/lib/CIME/XML/stream.py", line 22, in __init__
       GenericXML.__init__(self, infile, schema=schema)
     File "/glade/work/johnsonb/cesm2_1_3/cime/scripts/Tools/../../scripts/lib/CIME/XML/generic_xml.py", line 57, in __init__
       self.read(infile, schema)
     File "/glade/work/johnsonb/cesm2_1_3/cime/scripts/Tools/../../scripts/lib/CIME/XML/generic_xml.py", line 87, in read
       self.read_fd(fd)
     File "/glade/work/johnsonb/cesm2_1_3/cime/scripts/Tools/../../scripts/lib/CIME/XML/generic_xml.py", line 112, in read_fd
       self.tree = ET.parse(fd)
     File "/glade/u/home/johnsonb/miniconda2/lib/python2.7/xml/etree/ElementTree.py", line 1182, in parse
       tree.parse(source, parser)
     File "/glade/u/home/johnsonb/miniconda2/lib/python2.7/xml/etree/ElementTree.py", line 656, in parse
       parser.feed(data)
     File "/glade/u/home/johnsonb/miniconda2/lib/python2.7/xml/etree/ElementTree.py", line 1659, in feed
       self._raiseerror(v)
     File "/glade/u/home/johnsonb/miniconda2/lib/python2.7/xml/etree/ElementTree.py", line 1523, in _raiseerror
       raise err
   xml.etree.ElementTree.ParseError: junk after document element: line 4, column 0
   ERROR: /glade/work/johnsonb/cesm2_1_3/cime/src/components/data_comps/datm/cime_config/buildnml /glade/work/johnsonb/cases/G.benchmarking.f09_t13_e5 FAILED, see above
   ERROR: Case could not be built.


