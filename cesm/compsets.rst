########
Compsets
########

When defining non-default compsets, it is advisable to start with a default 
compset and adapt it.

The CESM `Component Set Definitions 
<https://www.cesm.ucar.edu/models/cesm2/config/compsets.html>`_ page lists all
of the defined compsets. 

In this case, there are two pairs of compsets whose confugration can be studied
as a starting point: the COREv2 forcing of `Large and Yeager (2009)
<https://doi.org/10.1007/s00382-008-0441-3>`_, which corresponds to the GIAF
and GIAF_HR compsets in CESM, and the JRA-55 forcing of `Kobayashi et al.
(2015) <https://doi.org/10.2151/jmsj.2015-001>`_, which corresponds to the
GIAF_JRA and GIAF_JRA_HR compsets.

- In **all** compsets, the land ice component is stub glacier.
- In **eddy-parameterizing** compsets, the wave component is Wave Watch 3.
- In **eddy-resolving** compsets, the wave component is stub wave.

GIAF
====

**Long name:** 2000_DATM%IAF_SLND_CICE_POP2_DROF%IAF_SGLC_WW3

+-----------------------+-----------------------+-----------------------+
|                       | Value                 | Description           |
+=======================+=======================+=======================+
| Initialization Time   | 2000                  | 1850: Pre-Industrial; |
|                       |                       | 2000 present day:     |
|                       |                       | Additional            |
|                       |                       | initialization times  |
|                       |                       | defined by            |
|                       |                       | components.           |
+-----------------------+-----------------------+-----------------------+
| Atmosphere            | DATM%IAF              | Data driven ATM       |
|                       |                       | COREv2 interannual    |
|                       |                       | forcing               |
+-----------------------+-----------------------+-----------------------+
| Land                  | SLND                  | Stub land component   |
+-----------------------+-----------------------+-----------------------+
| Sea-Ice               | CICE                  | Sea ICE (cice) model  |
|                       |                       | version 5             |
+-----------------------+-----------------------+-----------------------+
| Ocean                 | POP2                  | POP2                  |
+-----------------------+-----------------------+-----------------------+
| River runoff          | DROF%IAF              | Data runoff           |
|                       |                       | modelCOREv2           |
|                       |                       | interannual year      |
|                       |                       | forcing:              |
+-----------------------+-----------------------+-----------------------+
| Land Ice              | SGLC                  | Stub glacier (land    |
|                       |                       | ice) component        |
+-----------------------+-----------------------+-----------------------+
| Wave                  | WW3                   | Wave Watch            |
+-----------------------+-----------------------+-----------------------+

GIAF_HR
=======

**Long name:** 2000_DATM%IAF_SLND_CICE_POP2_DROF%IAF_SGLC_SWAV

+-----------------------+-----------------------+-----------------------+
|                       | Value                 | Description           |
+=======================+=======================+=======================+
| Initialization Time   | 2000                  | 1850: Pre-Industrial; |
|                       |                       | 2000 present day:     |
|                       |                       | Additional            |
|                       |                       | initialization times  |
|                       |                       | defined by            |
|                       |                       | components.           |
+-----------------------+-----------------------+-----------------------+
| Atmosphere            | DATM%IAF              | Data driven ATM       |
|                       |                       | COREv2 interannual    |
|                       |                       | forcing               |
+-----------------------+-----------------------+-----------------------+
| Land                  | SLND                  | Stub land component   |
+-----------------------+-----------------------+-----------------------+
| Sea-Ice               | CICE                  | Sea ICE (cice) model  |
|                       |                       | version 5             |
+-----------------------+-----------------------+-----------------------+
| Ocean                 | POP2                  | POP2                  |
+-----------------------+-----------------------+-----------------------+
| River runoff          | DROF%IAF              | Data runoff           |
|                       |                       | modelCOREv2           |
|                       |                       | interannual year      |
|                       |                       | forcing:              |
+-----------------------+-----------------------+-----------------------+
| Land Ice              | SGLC                  | Stub glacier (land    |
|                       |                       | ice) component        |
+-----------------------+-----------------------+-----------------------+
| Wave                  | SWAV                  | Stub wave component   |
+-----------------------+-----------------------+-----------------------+

GIAF_JRA
========

**Long name:** 2000_DATM%JRA-1p4-2018_SLND_CICE_POP2_DROF%JRA-1p4-2018_SGLC_WW3

+-----------------------+-----------------------+-----------------------+
|                       | Value                 | Description           |
+=======================+=======================+=======================+
| Initialization Time   | 2000                  | 1850: Pre-Industrial; |
|                       |                       | 2000 present day:     |
|                       |                       | Additional            |
|                       |                       | initialization times  |
|                       |                       | defined by            |
|                       |                       | components.           |
+-----------------------+-----------------------+-----------------------+
| Atmosphere            | DATM%JRA-1p4-2018     | Data driven ATM       |
|                       |                       | interannual JRA55     |
|                       |                       | forcing, v1.4,        |
|                       |                       | through 2018          |
+-----------------------+-----------------------+-----------------------+
| Land                  | SLND                  | Stub land component   |
+-----------------------+-----------------------+-----------------------+
| Sea-Ice               | CICE                  | Sea ICE (cice) model  |
|                       |                       | version 5             |
+-----------------------+-----------------------+-----------------------+
| Ocean                 | POP2                  | POP2                  |
+-----------------------+-----------------------+-----------------------+
| River runoff          | DROF%JRA-1p4-2018     | Data runoff           |
|                       |                       | modelJRA55            |
|                       |                       | interannual forcing,  |
|                       |                       | v1.4, through 2018    |
+-----------------------+-----------------------+-----------------------+
| Land Ice              | SGLC                  | Stub glacier (land    |
|                       |                       | ice) component        |
+-----------------------+-----------------------+-----------------------+
| Wave                  | WW3                   | Wave Watch            |
+-----------------------+-----------------------+-----------------------+

GIAF_JRA_HR
===========

**Long name:** 2000_DATM%JRA-1p4-2018_SLND_CICE%CICE4_POP2_DROF%JRA-1p4-2018_SGLC_SWAV

+-----------------------+-----------------------+-----------------------+
|                       | Value                 | Description           |
+=======================+=======================+=======================+
| Initialization Time   | 2000                  | 1850: Pre-Industrial; |
|                       |                       | 2000 present day:     |
|                       |                       | Additional            |
|                       |                       | initialization times  |
|                       |                       | defined by            |
|                       |                       | components.           |
+-----------------------+-----------------------+-----------------------+
| Atmosphere            | DATM%JRA-1p4-2018     | Data driven ATM       |
|                       |                       | interannual JRA55     |
|                       |                       | forcing, v1.4,        |
|                       |                       | through 2018          |
+-----------------------+-----------------------+-----------------------+
| Land                  | SLND                  | Stub land component   |
+-----------------------+-----------------------+-----------------------+
| Sea-Ice               | CICE%CICE4            | Sea ICE (cice) model  |
|                       |                       | version 5 :running    |
|                       |                       | with cice4 physics    |
+-----------------------+-----------------------+-----------------------+
| Ocean                 | POP2                  | POP2                  |
+-----------------------+-----------------------+-----------------------+
| River runoff          | DROF%JRA-1p4-2018     | Data runoff           |
|                       |                       | modelJRA55            |
|                       |                       | interannual forcing,  |
|                       |                       | v1.4, through 2018    |
+-----------------------+-----------------------+-----------------------+
| Land Ice              | SGLC                  | Stub glacier (land    |
|                       |                       | ice) component        |
+-----------------------+-----------------------+-----------------------+
| Wave                  | SWAV                  | Stub wave component   |
+-----------------------+-----------------------+-----------------------+

