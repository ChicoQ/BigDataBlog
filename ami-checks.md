## run the check script

```
[root@ip-10-205-227-133 prereq-checks-master]# ./prereq-check.sh
Cloudera Manager & CDH Prerequisites Checks v1.4.4

System information
-------------------
FQDN:          ip-10-205-227-133.airnz.co.nz
Distro:        Red Hat Enterprise Linux Server 7.4
Kernel:        3.10.0-693.5.2.el7.x86_64
CPUs:          1x Intel Xeon(R) E5-2676 v3 @ 2.40GHz
RAM:           1.79G
Disks:
               /dev/xvda  32.2 GB
Mount:
               /dev/xvda2  /                               xfs         rw,relatime,seclabel,attr2,inode64,noquota
Data mounts:
               None found
Free space:
               /opt      28G
               /var/log  28G
Cloudera RPMs: None installed
Timezone:      NZDT
DateTime:      Wed Nov 29 14:00:04 NZDT 2017
nsswitch:      files dns myhostname
DNS server:    10.205.227.200
10.205.227.168
10.205.227.136
Internet:      No

Prerequisite checks
-------------------
 PASS  System: /proc/sys/vm/swappiness should be 1
 PASS  System: tuned is not running
 PASS  System: tuned does not auto-start on boot
 PASS  System: /sys/kernel/mm/transparent_hugepage/defrag should be disabled
 PASS  System: SELinux should be disabled
 PASS  System: ntpd is running
 PASS  System: ntpd auto-starts on boot
 PASS  System: ntpd clock synced
 PASS  System: chronyd is not running
 FAIL  System: chronyd should not auto-start on boot
 PASS  System: Only 64bit packages should be installed
 PASS  System: bluetooth is not running
 PASS  System: bluetooth does not auto-start on boot
 PASS  System: cups is not running
 PASS  System: cups does not auto-start on boot
 PASS  System: ip6tables is not running
 PASS  System: ip6tables does not auto-start on boot
 PASS  System: postfix is not running
 PASS  System: postfix does not auto-start on boot
 PASS  System: /tmp mounted with noexec fails for CM versions older than 5.8.4, 5.9.2, and 5.10.0
 PASS  Network: IPv6 is not supported and must be disabled
 FAIL  Network: Computer name should be <= 15 characters (NetBIOS restriction)
 WARN  Network: /etc/hosts entries should be <= 2 (use DNS). Actual: 2, but has non localhost
 PASS  Network: nscd is running
 PASS  Network: nscd auto-starts on boot
 WARN  Network: sssd is not running
 WARN  Network: sssd does not auto-start on boot
 WARN  Network: 'dig' not found, skipping DNS checks. Run 'sudo yum install bind-utils' to fix.
 PASS  Network: firewalld is not running
 PASS  Network: firewalld does not auto-start on boot
 WARN  Database: MySQL server not installed, skipping version check
 PASS  Database: MySQL JDBC Driver is installed

```

## ntpd

```
[root@ip-10-205-227-133 ~]# ntpdc
ntpdc> peers
     remote           local      st poll reach  delay   offset    disp
=======================================================================
=10.65.5.20      10.205.227.133   3   64  377 0.02776 -0.000021 0.06345
*10.65.5.21      10.205.227.133   3   64  377 0.02792  0.000254 0.04503
ntpdc> quit

[root@ip-10-205-227-133 ~]# ntpq -np
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+10.65.5.20      192.168.10.70    3 u   31   64  377   27.764   -0.021   0.135
*10.65.5.21      192.168.10.72    3 u   39   64  377   27.930    0.254   0.030

===

[root@ip-10-205-44-115 ~]# ntpdc
ntpdc> peers
     remote           local      st poll reach  delay   offset    disp
=======================================================================
=ntp2.airnz.co.n 10.205.44.115    3 1024  377 0.02783  0.000079 0.12460
*ntp4.airnz.co.n 10.205.44.115    3 1024  377 0.02847  0.000203 0.13875
ntpdc> quit

[root@ip-10-205-44-115 ~]# ntpq -np
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
+10.65.5.20      192.168.10.70    3 u  115 1024  377   27.846    0.079   0.260
*10.65.5.21      192.168.10.72    3 u  860 1024  377   28.482    0.203   0.137

```

