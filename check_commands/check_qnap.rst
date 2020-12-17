Configuration of this command requires configuration of a NEMS service and the SNMP service on the QNAP device.  

QNAP configuration
==================
In the QNAP Control Panel go to Network & File Services -> SNMP and set the following.

- ``SNMP version:`` SNMP V1/V2
- ``Community`` public (or the community that matches the community in the NEMS Service configuration)

Then click Apply

NEMS Service Configuration 
==========================
In the NEMS NConf click on > Services -> Add
Set the following.

- ``Service Name`` Check QNAP (or what ever you like)
- ``Check Command`` check_qnap
- ``check period`` 24x7 (or desired configuration)
- ``notification period`` 24x7 (or desired configuration)
- ``max check attempts`` 3 (or desired configuration)
- ``check interval`` 10 (or desired configuration)
- ``retry interval`` 1 (or desired configuration)
- ``first notification delay`` 5 (or desired configuration)
- ``notification interval`` 20 (or desired configuration)
- ``notification options`` c (or desired configuration)
- ``ARG1`` public (must match community setting in QNAP configuration)
- ``ARG2`` diskused (see selection from below)
- ``ARG3`` 80 (warning value)
- ``ARG4`` 95 (critical value)

click Submit, Generate the Nagios Config and then deploy


Check Command: check_qnap
=========================

-  ``cpuload`` system CPU load in percent
-  ``cputemp`` system CPU Temperature
-  ``systemp`` system Temperature
-  ``hdtemp`` Harddive Temperature
-  ``diskused`` diskusage in perent
-  ``mem`` memory usage in perent
-  ``volstatus`` Volume status
-  ``fan`` Fan Speed
-  ``hdstatus`` Harddive status
-  ``cachediskstatus`` Cachedisk status
-  ``lunstatus`` LUN status
-  ``raidstatus`` RAID status
-  ``powerstatus`` Power status
-  ``sysinfo`` Output information about the QNAP device

Thanks to ChrisD for `contributing to the documentation <https://forum.nemslinux.com/viewtopic.php?f=44&t=764>`__ for this check command.
