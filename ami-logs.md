
[root@ip-10-205-227-133 ~]# ./prereq-check-single.sh
Cloudera Manager & CDH Prerequisites Checks v1.0.1

System information
-------------------
FQDN:          ip-10-205-227-133.airnz.co.nz
Distro:        Red Hat Enterprise Linux Server 7.4 (Maipo)
Kernel:        3.10.0-693.el7.x86_64
CPUs:          1x Intel Xeon(R) E5-2676 v3 @ 2.40GHz
RAM:           1 GB
Disks:
               /dev/xvda  32.2 GB
Data mounts:   None found
Free space:
               /opt
               /var/log
Cloudera RPMs: None installed
Timezone:      Pacific/Auckland
DateTime:      Wed Nov 29 10:44:16 NZDT 2017
nsswitch:      files dns myhostname
DNS server:    10.205.227.200

Prerequisite checks
-------------------
 FAIL  System: /proc/sys/vm/swappiness should be 1. Actual: 30
 FAIL  System: /sys/kernel/mm/transparent_hugepage/defrag should be disabled. Actual: always
 PASS  System: SELinux should be disabled
 FAIL  System: ulimits too low. E.g. nofile for 'hdfs' should be >= 32768. Actual: 1024
 FAIL  System: ntpd is not running

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

 FAIL  System: ntpd does not auto-start on boot
 WARN  Network: No Internet connection
 PASS  Network: /etc/hosts entries should be <= 2 (use DNS). Actual: 2
 PASS  Network: iptables is not installed

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

 PASS  Network: iptables does not auto-start on boot
 FAIL  Network: nscd is not installed

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

 FAIL  Network: nscd does not auto-start on boot
 PASS  Network: sssd is not installed

Note: This output shows SysV services only and does not include native
      systemd services. SysV configuration data might be overridden by native
      systemd configuration.

      If you want to list systemd services use 'systemctl list-unit-files'.
      To see services enabled on particular target use
      'systemctl list-dependencies [target]'.

 PASS  Network: sssd does not auto-start on boot
 FAIL  Java: Oracle Java not installed
 FAIL  Java: Unsupported Java versions installed:
       - java-1.8.0-openjdk-1.8.0.151-1.b12.el7_4.x86_64
       - java-1.8.0-openjdk-headless-1.8.0.151-1.b12.el7_4.x86_64
 FAIL  Database: Supported MySQL server installed. Actual: mysql-connector-java-5.1.25
 PASS  Database: MySQL connector is installed
 PASS  Hostname: Format looks okay

### Additional Checks for NON PROD Environment [Checked By RAM] ###

 PASS  /var/log Mount Size: Looks OK [30 G].
 PASS  /var/lib Mount Size: Looks OK [30 G].
 PASS  /tmp Mount Size: Looks OK [30 G].
 PASS  /opt Mount Size: Looks OK [30 G].
 PASS  Ulimit FSize: Looks OK. NO Entry for fsize.

===

##

[root@ip-10-205-44-115 ~]# cat /etc/ntp.conf
# For more information about this file, see the man pages
# ntp.conf(5), ntp_acc(5), ntp_auth(5), ntp_clock(5), ntp_misc(5), ntp_mon(5).

driftfile /var/lib/ntp/drift

# Permit time synchronization with our time source, but do not
# permit the source to query or modify the service on this system.
restrict default kod nomodify notrap nopeer noquery
restrict -6 default kod nomodify notrap nopeer noquery

# Permit all access over the loopback interface.  This could
# be tightened as well, but to do so would effect some of
# the administrative functions.
restrict 127.0.0.1
restrict ::1

# Hosts on local network are less restricted.
#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap

# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
server ntp2.airnz.co.nz
server ntp4.airnz.co.nz

#broadcast 192.168.1.255 autokey        # broadcast server
#broadcastclient                        # broadcast client
#broadcast 224.0.1.1 autokey            # multicast server
#multicastclient 224.0.1.1              # multicast client
#manycastserver 239.255.254.254         # manycast server
#manycastclient 239.255.254.254 autokey # manycast client

# Enable public key cryptography.
#crypto

includefile /etc/ntp/crypto/pw

# Key file containing the keys and key identifiers used when operating
# with symmetric key cryptography.
keys /etc/ntp/keys

# Specify the key identifiers which are trusted.
#trustedkey 4 8 42

# Specify the key identifier to use with the ntpdc utility.
#requestkey 8

# Specify the key identifier to use with the ntpq utility.
#controlkey 8

# Enable writing of statistics records.
#statistics clockstats cryptostats loopstats peerstats

# Disable the monitoring facility to prevent amplification attacks using ntpdc
# monlist command when default restrict does not include the noquery flag. See
# CVE-2013-5211 for more details.
# Note: Monitoring will not be disabled with the limited restriction flag.
disable monitor

===
[root@ip-10-205-227-133 ~]# cat /etc/ntp.conf
# For more information about this file, see the man pages
# ntp.conf(5), ntp_acc(5), ntp_auth(5), ntp_clock(5), ntp_misc(5), ntp_mon(5).

driftfile /var/lib/ntp/drift

# Permit time synchronization with our time source, but do not
# permit the source to query or modify the service on this system.
restrict default nomodify notrap nopeer noquery

# Permit all access over the loopback interface.  This could
# be tightened as well, but to do so would effect some of
# the administrative functions.
restrict 127.0.0.1
restrict ::1

# Hosts on local network are less restricted.
#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap

# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).
#server 0.rhel.pool.ntp.org iburst
#server 1.rhel.pool.ntp.org iburst
#server 2.rhel.pool.ntp.org iburst
#server 3.rhel.pool.ntp.org iburst

server ntp2.airnz.co.nz
server ntp4.airnz.co.nz

#broadcast 192.168.1.255 autokey        # broadcast server
#broadcastclient                        # broadcast client
#broadcast 224.0.1.1 autokey            # multicast server
#multicastclient 224.0.1.1              # multicast client
#manycastserver 239.255.254.254         # manycast server
#manycastclient 239.255.254.254 autokey # manycast client

# Enable public key cryptography.
#crypto

includefile /etc/ntp/crypto/pw

# Key file containing the keys and key identifiers used when operating
# with symmetric key cryptography.
keys /etc/ntp/keys

# Specify the key identifiers which are trusted.
#trustedkey 4 8 42

# Specify the key identifier to use with the ntpdc utility.
#requestkey 8

# Specify the key identifier to use with the ntpq utility.
#controlkey 8

# Enable writing of statistics records.
#statistics clockstats cryptostats loopstats peerstats

# Disable the monitoring facility to prevent amplification attacks using ntpdc
# monlist command when default restrict does not include the noquery flag. See
# CVE-2013-5211 for more details.
# Note: Monitoring will not be disabled with the limited restriction flag.
disable monitor
server 10.65.5.20   # added by /sbin/dhclient-script
server 10.65.5.21   # added by /sbin/dhclient-script




[root@ip-10-205-227-133 ~]# service ntpd start
Redirecting to /bin/systemctl start ntpd.service
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# service ntpd status
Redirecting to /bin/systemctl status ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 10:50:17 NZDT; 5s ago
  Process: 21167 ExecStart=/usr/sbin/ntpd -u ntp:ntp $OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 21168 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─21168 /usr/sbin/ntpd -u ntp:ntp -g

Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: 0.0.0.0 c01d 0d kern kernel time sync enabled
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: ntp_io: estimated max descriptors: 1024, initial socket boundary: 16
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: Listen and drop on 1 v6wildcard :: UDP 123
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: Listen normally on 2 lo 127.0.0.1 UDP 123
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: Listen normally on 3 eth0 10.205.227.133 UDP 123
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: Listening on routing socket on fd #20 for interface updates
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: 0.0.0.0 c016 06 restart
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: 0.0.0.0 c012 02 freq_set kernel 0.000 PPM
Nov 29 10:50:17 ip-10-205-227-133.airnz.co.nz ntpd[21168]: 0.0.0.0 c011 01 freq_not_set
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl enable ntpd
Created symlink from /etc/systemd/system/multi-user.target.wants/ntpd.service to /usr/lib/systemd/system/ntpd.service.
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# ntpdc
ntpdc> peer
     remote           local      st poll reach  delay   offset    disp
=======================================================================
=10.65.5.20      10.205.227.133   3   64    1 0.02783 -0.000095 2.81735
=10.65.5.21      10.205.227.133   3   64    1 0.02789 -0.000109 2.81735
ntpdc>
ntpdc> peers
     remote           local      st poll reach  delay   offset    disp
=======================================================================
=10.65.5.20      10.205.227.133   3   64    1 0.02783 -0.000095 2.81735
=10.65.5.21      10.205.227.133   3   64    1 0.02789 -0.000109 2.81735
ntpdc>
ntpdc> quit
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# ntpq -np
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 10.65.5.20      192.168.10.72    3 u    3   64    3   27.719   -1.175   1.080
 10.65.5.21      192.168.10.70    3 u    4   64    3   27.947   -1.169   1.059
[root@ip-10-205-227-133 ~]#

yum -y install sendmail

[root@ip-10-205-227-133 ~]# yum list installed | grep net-tools
net-tools.x86_64              2.0-0.22.20131004git.el7
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# yum list installed | grep crontab
crontabs.noarch               1.11-6.20121102git.el7
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl start tuned
[root@ip-10-205-227-133 ~]# tuned-adm off
[root@ip-10-205-227-133 ~]# tuned-adm list
Available profiles:
- balanced                    - General non-specialized tuned profile
- desktop                     - Optimize for the desktop use-case
- latency-performance         - Optimize for deterministic performance at the cost of increased power consumption
- network-latency             - Optimize for deterministic performance at the cost of increased power consumption, focused on low latency network performance
- network-throughput          - Optimize for streaming network throughput, generally only necessary on older CPUs or 40G+ networks
- powersave                   - Optimize for low power consumption
- throughput-performance      - Broadly applicable tuning that provides excellent performance across a variety of common server workloads
- virtual-guest               - Optimize for running inside a virtual guest
- virtual-host                - Optimize for running KVM guests
No current active profile.
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl stop tuned
[root@ip-10-205-227-133 ~]# systemctl disable tuned
Removed symlink /etc/systemd/system/multi-user.target.wants/tuned.service.
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# echo never > /sys/kernel/mm/transparent_hugepage/enabled
[root@ip-10-205-227-133 ~]# echo never > /sys/kernel/mm/transparent_hugepage/defrag
[root@ip-10-205-227-133 ~]# ll /etc/rc.d/rc.local
-rw-r--r--. 1 root root 473 Jun 27 23:12 /etc/rc.d/rc.local
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# chmod +x /etc/rc.d/rc.local
[root@ip-10-205-227-133 ~]# ll /etc/rc.d/rc.local
-rwxr-xr--. 1 root root 473 Jun 27 23:12 /etc/rc.d/rc.local
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# vim /etc/rc.d/rc.local
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /etc/rc.d/rc.local
#!/bin/bash
# THIS FILE IS ADDED FOR COMPATIBILITY PURPOSES
#
# It is highly advisable to create own systemd services or udev rules
# to run scripts during boot instead of using this file.
#
# In contrast to previous versions due to parallel execution during boot
# this script will NOT be run after all other services.
#
# Please note that you must run 'chmod +x /etc/rc.d/rc.local' to ensure
# that this script will be executed during boot.

touch /var/lock/subsys/local
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
[root@ip-10-205-227-133 ~]#

===

[root@ip-10-205-227-133 ~]# cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /sys/kernel/mm/transparent_hugepage/defrag
always madvise [never]

===

[root@ip-10-205-227-133 ~]# cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /sys/kernel/mm/transparent_hugepage/defrag
always madvise [never]
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /proc/sys/vm/swappiness
60
[root@ip-10-205-227-133 ~]# sysctl -w vm.swappiness=1
vm.swappiness = 1
[root@ip-10-205-227-133 ~]# cat /proc/sys/vm/swappiness
1

===

[root@ip-10-205-227-133 ~]# getenforce
Permissive
[root@ip-10-205-227-133 ~]# vim /etc/selinux/config
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /etc/selinux/config

# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=permissive
# SELINUXTYPE= can take one of three two values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted


[root@ip-10-205-227-133 ~]# setenforce 0
[root@ip-10-205-227-133 ~]# getenforce
Permissive

===

[root@ip-10-205-227-133 ~]# yum list installed | grep nscd
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# yum install -y nscd
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Failed to get region name from EC2
Resolving Dependencies
--> Running transaction check
---> Package nscd.x86_64 0:2.17-196.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

=============================================================================================================================================================================

 Package                         Arch                              Version                                 Repository                                                   Size =============================================================================================================================================================================

Installing:
 nscd                            x86_64                            2.17-196.el7                            rhui-REGION-rhel-server-releases                            273 k

Transaction Summary
=============================================================================================================================================================================

Install  1 Package

Total download size: 273 k
Installed size: 184 k
Downloading packages:
nscd-2.17-196.el7.x86_64.rpm                                                                                                                          | 273 kB  00:00:00

Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : nscd-2.17-196.el7.x86_64                                                                                                                                  1/1

  Verifying  : nscd-2.17-196.el7.x86_64                                                                                                                                  1/1


Installed:
  nscd.x86_64 0:2.17-196.el7


Complete!
[root@ip-10-205-227-133 ~]# service nscd status
Redirecting to /bin/systemctl status nscd.service
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
[root@ip-10-205-227-133 ~]# systemctl service nscd
Unknown operation 'service'.
[root@ip-10-205-227-133 ~]# systemctl start nscd
[root@ip-10-205-227-133 ~]# systemctl status nscd
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 11:04:44 NZDT; 17s ago
  Process: 21501 ExecStart=/usr/sbin/nscd $NSCD_OPTIONS (code=exited, status=0/SUCCESS)
 Main PID: 21502 (nscd)
   CGroup: /system.slice/nscd.service
           └─21502 /usr/sbin/nscd

Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring file `/etc/hosts` (4)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring directory `/etc` (2)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring file `/etc/resolv.conf` (5)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring directory `/etc` (2)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring file `/etc/services` (6)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 monitoring directory `/etc` (2)
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 disabled inotify-based monitoring for file `/etc/netgroup': No such file or directory
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 stat failed for file `/etc/netgroup'; will try again later: No such file or directory
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz nscd[21502]: 21502 Access Vector Cache (AVC) started
Nov 29 11:04:44 ip-10-205-227-133.airnz.co.nz systemd[1]: Started Name Service Cache Daemon.
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl enable nscd
Created symlink from /etc/systemd/system/multi-user.target.wants/nscd.service to /usr/lib/systemd/system/nscd.service.
Created symlink from /etc/systemd/system/sockets.target.wants/nscd.socket to /usr/lib/systemd/system/nscd.socket.

===

