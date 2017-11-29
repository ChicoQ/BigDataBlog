
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
[root@ip-10-205-227-133 ~]# yum install -y wget
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Failed to get region name from EC2
Package wget-1.14-15.el7_4.1.x86_64 already installed and latest version
Nothing to do
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.43.tar.gz
--2017-11-29 11:08:34--  https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.43.tar.gz
Resolving internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com (internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com)... 10.205.227.218, 10.205.227.188, 10.205.227.138
Connecting to internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com (internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com)|10.205.227.218|:3128... connected.
Proxy tunneling failed: ForbiddenUnable to establish SSL connection.
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# ll
total 32
-rw-------. 1 root root  7497 Jul 12 04:11 anaconda-ks.cfg
-rw-------. 1 root root  6689 Jul 12 04:11 original-ks.cfg
-rwxr-x---. 1 root root 12795 Nov 29 10:44 prereq-check-single.sh
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# wget http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44-sources.jar
--2017-11-29 11:09:36--  http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.44/mysql-connector-java-5.1.44-sources.jar
Resolving internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com (internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com)... 10.205.227.218, 10.205.227.188, 10.205.227.138
Connecting to internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com (internal-prod-core-applicat-m9rtj9hdhdea-2131951945.ap-southeast-2.elb.amazonaws.com)|10.205.227.218|:3128... connected.
Proxy request sent, awaiting response... 403 Forbidden
2017-11-29 11:09:36 ERROR 403: Forbidden.

===
https://mvnrepository.com/artifact/mysql/mysql-connector-java/5.1.44

scp -i Downloads\DirectorProd1.pem Downloads\mysql-connector-java-5.1.44.jar cloudera-scm@10.205.227.133:~


##

mv /home/cloudera-scm/Downloads\\jce_policy-8.zip jce_policy-8.zip
mv /home/cloudera-scm/Downloads\\jdk-8u144-linux-x64.tar.gz jdk-8u144-linux-x64.tar.gz

tar xvzf jdk-8u144-linux-x64.tar.gz -C /usr/java/

[root@ip-10-205-227-133 ~]# chmod -R 777 /usr/java/jdk1.8.0_144/
[root@ip-10-205-227-133 ~]# chown -R root:root /usr/java/jdk1.8.0_144/

[root@ip-10-205-227-133 ~]# cat /etc/profile
# /etc/profile

# System wide environment and startup programs, for login setup
# Functions and aliases go in /etc/bashrc

# It's NOT a good idea to change this file unless you know what you
# are doing. It's much better to create a custom.sh shell script in
# /etc/profile.d/ to make custom changes to your environment, as this
# will prevent the need for merging in future updates.

pathmunge () {
    case ":${PATH}:" in
        *:"$1":*)
            ;;
        *)
            if [ "$2" = "after" ] ; then
                PATH=$PATH:$1
            else
                PATH=$1:$PATH
            fi
    esac
}


if [ -x /usr/bin/id ]; then
    if [ -z "$EUID" ]; then
        # ksh workaround
        EUID=`/usr/bin/id -u`
        UID=`/usr/bin/id -ru`
    fi
    USER="`/usr/bin/id -un`"
    LOGNAME=$USER
    MAIL="/var/spool/mail/$USER"
fi

# Path manipulation
if [ "$EUID" = "0" ]; then
    pathmunge /usr/sbin
    pathmunge /usr/local/sbin
else
    pathmunge /usr/local/sbin after
    pathmunge /usr/sbin after
fi

HOSTNAME=`/usr/bin/hostname 2>/dev/null`
HISTSIZE=1000
if [ "$HISTCONTROL" = "ignorespace" ] ; then
    export HISTCONTROL=ignoreboth
else
    export HISTCONTROL=ignoredups
fi

export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL

# By default, we want umask to get set. This sets it for login shell
# Current threshold for system reserved uid/gids is 200
# You could check uidgid reservation validity in
# /usr/share/doc/setup-*/uidgid file
if [ $UID -gt 199 ] && [ "`/usr/bin/id -gn`" = "`/usr/bin/id -un`" ]; then
    umask 002
else
    umask 022
fi

for i in /etc/profile.d/*.sh ; do
    if [ -r "$i" ]; then
        if [ "${-#*i}" != "$-" ]; then
            . "$i"
        else
            . "$i" >/dev/null
        fi
    fi
done

unset i
unset -f pathmunge
export http_proxy=internal-prod-core-Applicat-M9RTJ9HDHDEA-2131951945.ap-southeast-2.elb.amazonaws.com:3128
export https_proxy=internal-prod-core-Applicat-M9RTJ9HDHDEA-2131951945.ap-southeast-2.elb.amazonaws.com:3128

export JAVA_HOME=/usr/java/jdk1.8.0_144
export PATH=$JAVA_HOME/bin:$PATH

===
[root@ip-10-205-227-133 ~]# ll
total 182192
-rw-------. 1 root root      7497 Jul 12 04:11 anaconda-ks.cfg
-rw-r-----. 1 root root      8409 Nov 29 13:16 jce_policy-8.zip
-rw-r-----. 1 root root 185515842 Nov 29 13:16 jdk-8u144-linux-x64.tar.gz
-rw-r-----. 1 root root    999632 Nov 29 11:12 mysql-connector-java-5.1.44.jar
-rw-------. 1 root root      6689 Jul 12 04:11 original-ks.cfg
-rwxr-x---. 1 root root     12795 Nov 29 10:44 prereq-check-single.sh
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# mv mysql-connector-java-5.1.44.jar /usr/share/java

[root@ip-10-205-227-133 java]# ln -s mysql-connector-java-5.1.44.jar mysql-connector-java.jar
[root@ip-10-205-227-133 java]# ll mysql-connector-java*
-rw-r-----. 1 root root 999632 Nov 29 11:12 mysql-connector-java-5.1.44.jar
lrwxrwxrwx. 1 root root     31 Nov 29 13:28 mysql-connector-java.jar -> mysql-connector-java-5.1.44.jar
-rw-r--r--. 1 root root 883899 Dec 29  2013 mysql-connector-java.jar.old

===

[root@ip-10-205-227-133 java]# yum install -y unzip
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Failed to get region name from EC2
cloudera-director                                                                                                                      |  951 B  00:00:00

cloudera-manager                                                                                                                       |  951 B  00:00:00

rhui-REGION-client-config-server-7                                                                                                     | 2.9 kB  00:00:00

rhui-REGION-rhel-server-releases                                                                                                       | 3.5 kB  00:00:00

rhui-REGION-rhel-server-rh-common                                                                                                      | 3.8 kB  00:00:00

Resolving Dependencies
--> Running transaction check
---> Package unzip.x86_64 0:6.0-16.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

==============================================================================================================================================================

 Package                      Arch                          Version                             Repository                                               Size ==============================================================================================================================================================

Installing:
 unzip                        x86_64                        6.0-16.el7                          rhui-REGION-rhel-server-releases                        169 k

Transaction Summary
==============================================================================================================================================================

Install  1 Package

Total download size: 169 k
Installed size: 365 k
Downloading packages:
unzip-6.0-16.el7.x86_64.rpm                                                                                                            | 169 kB  00:00:00

Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : unzip-6.0-16.el7.x86_64                                                                                                                    1/1

  Verifying  : unzip-6.0-16.el7.x86_64                                                                                                                    1/1


Installed:
  unzip.x86_64 0:6.0-16.el7


Complete!
[root@ip-10-205-227-133 java]# cd ~
[root@ip-10-205-227-133 ~]# ll
total 181212
-rw-------. 1 root root      7497 Jul 12 04:11 anaconda-ks.cfg
-rw-r-----. 1 root root      8409 Nov 29 13:16 jce_policy-8.zip
-rw-r-----. 1 root root 185515842 Nov 29 13:16 jdk-8u144-linux-x64.tar.gz
-rw-------. 1 root root      6689 Jul 12 04:11 original-ks.cfg
-rwxr-x---. 1 root root     12795 Nov 29 10:44 prereq-check-single.sh

[root@ip-10-205-227-133 ~]# unzip jce_policy-8.zip
Archive:  jce_policy-8.zip
   creating: UnlimitedJCEPolicyJDK8/
  inflating: UnlimitedJCEPolicyJDK8/local_policy.jar
  inflating: UnlimitedJCEPolicyJDK8/README.txt
  inflating: UnlimitedJCEPolicyJDK8/US_export_policy.jar
[root@ip-10-205-227-133 ~]# ll
total 181212
-rw-------. 1 root root      7497 Jul 12 04:11 anaconda-ks.cfg
-rw-r-----. 1 root root      8409 Nov 29 13:16 jce_policy-8.zip
-rw-r-----. 1 root root 185515842 Nov 29 13:16 jdk-8u144-linux-x64.tar.gz
-rw-------. 1 root root      6689 Jul 12 04:11 original-ks.cfg
-rwxr-x---. 1 root root     12795 Nov 29 10:44 prereq-check-single.sh
drwxrwxr-x. 2 root root        76 Dec 21  2013 UnlimitedJCEPolicyJDK8

[root@ip-10-205-227-133 ~]# ll UnlimitedJCEPolicyJDK8/
total 16
-rw-rw-r--. 1 root root 3035 Dec 21  2013 local_policy.jar
-rw-r--r--. 1 root root 7323 Dec 21  2013 README.txt
-rw-rw-r--. 1 root root 3023 Dec 21  2013 US_export_policy.jar

[root@ip-10-205-227-133 ~]# cd UnlimitedJCEPolicyJDK8/

[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]# cp local_policy.jar /usr/java/jdk1.8.0_144/jre/lib/security/
cp: overwrite ‘/usr/java/jdk1.8.0_144/jre/lib/security/local_policy.jar’? y
[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]#
[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]# cp US_export_policy.jar /usr/java/jdk1.8.0_144/jre/lib/security/
cp: overwrite ‘/usr/java/jdk1.8.0_144/jre/lib/security/US_export_policy.jar’? y

[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]# $JAVA_HOME/bin/jrunscript -e 'print (javax.crypto.Cipher.getMaxAllowedKeyLength("RC5") >= 256);'
-bash: /bin/jrunscript: No such file or directory
[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]#
[root@ip-10-205-227-133 UnlimitedJCEPolicyJDK8]# echo $JAVA_HOME

## create new console

[root@ip-10-205-227-133 ~]# $JAVA_HOME/bin/jrunscript -e 'print (javax.crypto.Cipher.getMaxAllowedKeyLength("RC5") >= 256);'
true

##

[root@ip-10-205-227-133 ~]# yum update

[root@ip-10-205-227-133 ~]# reboot

===
[cloudera-scm@ip-10-205-227-133 ~]$ sudo su -
Last login: Wed Nov 29 13:32:58 NZDT 2017 on pts/1
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
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
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# service ntpd status
Redirecting to /bin/systemctl status ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: inactive (dead)
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# ntpdc
ntpdc> peers
ntpdc: read: Connection refused
ntpdc>
ntpdc>
ntpdc> quit
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# service ntpd start
Redirecting to /bin/systemctl start ntpd.service
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl enable ntpd
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# service ntpd status
Redirecting to /bin/systemctl status ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 13:48:34 NZDT; 26s ago
 Main PID: 10623 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─10623 /usr/sbin/ntpd -u ntp:ntp -g

Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c01d 0d kern kernel time sync enabled
Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz systemd[1]: Started Network Time Service.
Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz ntpd[10623]: ntp_io: estimated max descriptors: 1024, initial socket boundary: 16
Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen and drop on 1 v6wildcard :: UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen normally on 2 lo 127.0.0.1 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen normally on 3 eth0 10.205.227.133 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listening on routing socket on fd #20 for interface updates
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c016 06 restart
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c012 02 freq_set kernel -16.938 PPM
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# ntpdc
ntpdc> peers
     remote           local      st poll reach  delay   offset    disp
=======================================================================
=10.65.5.20      10.205.227.133   3   64    1 0.02766 -0.000058 2.81735
=10.65.5.21      10.205.227.133   3   64    1 0.02805  0.000191 2.81735
ntpdc>
ntpdc> quit
[root@ip-10-205-227-133 ~]# ntpq -np
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 10.65.5.20      192.168.10.70    3 u   57   64    1   27.676   -0.058   0.000
 10.65.5.21      192.168.10.72    3 u   56   64    1   28.055    0.191   0.000
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /proc/sys/vm/swappiness
60
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# sysctl -w vm.swappiness=1
vm.swappiness = 1
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# cat /proc/sys/vm/swappiness
1
[root@ip-10-205-227-133 ~]#
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


[root@ip-10-205-227-133 ~]# getenforce
Permissive
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# service nscd status
Redirecting to /bin/systemctl status nscd.service
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 13:39:59 NZDT; 11min ago
 Main PID: 522 (nscd)
   CGroup: /system.slice/nscd.service
           └─522 /usr/sbin/nscd

Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 monitoring directory `/etc` (2)
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 monitoring file `/etc/services` (6)
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 monitoring directory `/etc` (2)
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 disabled inotify-based monitoring for file `/etc/netgroup': No such file or directory
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 stat failed for file `/etc/netgroup'; will try again later: No such file or directory
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 Access Vector Cache (AVC) started
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz systemd[1]: Started Name Service Cache Daemon.
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 monitored file `/etc/resolv.conf` was moved into place, adding watch
Nov 29 13:39:59 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 ignored inotify event for `/etc/resolv.conf` (file exists)
Nov 29 13:40:18 ip-10-205-227-133.airnz.co.nz nscd[522]: 522 checking for monitored file `/etc/netgroup': No such file or directory
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# systemctl status tuned
● tuned.service - Dynamic System Tuning Daemon
   Loaded: loaded (/usr/lib/systemd/system/tuned.service; disabled; vendor preset: enabled)
   Active: inactive (dead)
[root@ip-10-205-227-133 ~]#
[root@ip-10-205-227-133 ~]# $JAVA_HOME/bin/jrunscript -e 'print (javax.crypto.Cipher.getMaxAllowedKeyLength("RC5") >= 256);'
true

===

[root@ip-10-205-227-133 ~]# ./prereq-check-single.sh
Cloudera Manager & CDH Prerequisites Checks v1.0.1

System information
-------------------
FQDN:          ip-10-205-227-133.airnz.co.nz
Distro:        Red Hat Enterprise Linux Server 7.4 (Maipo)
Kernel:        3.10.0-693.5.2.el7.x86_64
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
DateTime:      Wed Nov 29 13:53:26 NZDT 2017
nsswitch:      files dns myhostname
DNS server:    10.205.227.200

Prerequisite checks
-------------------
 PASS  System: /proc/sys/vm/swappiness should be 1
 PASS  System: /sys/kernel/mm/transparent_hugepage/defrag should be disabled
 PASS  System: SELinux should be disabled
 FAIL  System: ulimits too low. E.g. nofile for 'hdfs' should be >= 32768. Actual: 1024
 PASS  System: ntpd is running

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
 PASS  Network: nscd is running

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
scp -i DirectorProd1.pem prereq-checks-master.zip cloudera-scm@10.205.227.133:~






