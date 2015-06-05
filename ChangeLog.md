=== Version 2.04 === (IN SVN)

  * MAX LATENCY must be stated in seconds not in miliseconds.

### Version 2.03 ###

Modifications made by Gaven Henry.

  * COOLDOWNDELAY - deprecated use COOLDOWNDELAY01 and COOLDOWNDELAY02
  * COOLDOWNDELAY01 - The period of time to wait after a FAILOVER event before commencing monitoring again
  * COOLDOWNDELAY02 - The period of time to wait after a FAILBACK even before commencing monitoring again
  * FIX: Email alerts were sent before routes were changed on failover / failback. Moved to generate and send alerts after the route switch so that routing table included in email is correct
  * FIX: Some notices echoing to CLI even when QUIET=1, redirected to log file
  * ADD: README with description of variables and info now included in package & SVN
  * ADD: Changelog now included in package & SVN

### Version 2.02 (in SVN) ###

  * WFS now checks the maximum latency as specfied in miliseconds. If this threshold is exceeded, a failover is performed. This may be useful for setups where the latency of a connection is as important as the connection itself. Low latency connections for VOIP for example.

### Version 2.01 ###

  * WFS permits the user to execute (external) commands when a failover or restore event occurs. This feature did not work properly and has been fixed within this version. You can specify commands to be executed within the config file. Consult the manual for additional information.

### Version 2.0 ###

Initial release of the 2.x branch.