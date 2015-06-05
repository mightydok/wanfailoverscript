For those with a backup internet connection that are looking for automatic monitoring and failover, WFS may be of interest to you. WFS is a Bash script written for Linux (IP-tables) based routers. WFS can be used to:

1. automatic failover to a backup internet connection;
2. automatic failover to a backup WAN connection.

WFS automatically switches between your primary WAN connection and backup / failover WAN connection when the primary WAN connection is no longer available.

After a failover to your backup WAN connection, WFS continues to monitor if your primary WAN connection is available again. When the primary connection is working again, it automatically switches back to your primary WAN connection.

WFS monitors the availability of your WAN connection by periodically sending ICMP requests (PING) to one or more selected hosts. The check interval and hosts can be configured.

WFS switches between WAN connections by changing the default gateway to which all regular traffic is sent. It has no dependencies and probably works on any Linux system.

Feedback is always appreciated. Please e-mail me at louwrentius gmail com if you use it at your site, would like to hear from it and your experience with the script.