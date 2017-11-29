# AMI test result

## yum related

### yum update

### yum install list
- sendmail
- unzip
- ntp
- net-tools
- crontab
- nscd
- wget

## related services

### ntpd

- add following ntp servers to /etc/ntp.conf
```
server ntp2.airnz.co.nz
server ntp4.airnz.co.nz
```

- `service ntpd start`
- `systemctl enable ntpd`

```
[root@ip-10-205-227-133 ~]# systemctl status ntpd
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 13:48:34 NZDT; 2h 15min ago
 Main PID: 10623 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─10623 /usr/sbin/ntpd -u ntp:ntp -g

Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz systemd[1]: Started Network Time Service.
Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz ntpd[10623]: ntp_io: estimated max descriptors: 1024, initial socket boundary: 16
Nov 29 13:48:34 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen and drop on 1 v6wildcard :: UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen normally on 2 lo 127.0.0.1 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listen normally on 3 eth0 10.205.227.133 UDP 123
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: Listening on routing socket on fd #20 for interface updates
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c016 06 restart
Nov 29 13:48:35 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c012 02 freq_set kernel -16.938 PPM
Nov 29 13:51:52 ip-10-205-227-133.airnz.co.nz ntpd[10623]: 0.0.0.0 c615 05 clock_sync
```

### disable the tuned service 
https://www.cloudera.com/documentation/enterprise/latest/topics/cdh_admin_performance.html#cdh_performance__disable-tuned
```
systemctl start tuned
tuned-adm off
tuned-adm list
systemctl stop tuned
systemctl disable tuned
```

### Disabling Transparent Hugepages
add the following commands to the `/etc/rc.d/rc.local` file

```
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```
Modify the permissions of the rc.local file

`chmod +x /etc/rc.d/rc.local`

### nscd
```
systemctl start nscd
systemctl enable nscd
```

```
[root@ip-10-205-227-133 ~]# systemctl status nscd
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; enabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-11-29 13:39:59 NZDT; 2h 19min ago
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
```

### Disabling SELinux
https://www.cloudera.com/documentation/enterprise/latest/topics/install_cdh_disable_selinux.html

- Open the `/etc/selinux/config` file
- Change the line `SELINUX=enforcing` to `SELINUX=permissive`
- Save and close the file
- Restart your system or run the following command to disable SELinux immediately
`setenforce 0`

## java related

- downloaded `jce_policy-8.zip` and `jdk-8u144-linux-x64.tar.gz` from Oracle website and untar under `/usr/java/`
- add following to `/etc/profile`
```
export JAVA_HOME=/usr/java/jdk1.8.0_144
export PATH=$JAVA_HOME/bin:$PATH
```

```
[root@ip-10-205-227-133 ~]# java -version
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```

## mysql driver

- download `mysql-connector-java-5.1.44.jar` from https://mvnrepository.com/artifact/mysql/mysql-connector-java/5.1.44
- copied above jar file into `/usr/share/java` and created a link
```
ln -s mysql-connector-java-5.1.44.jar mysql-connector-java.jar
```

## final result

```
[root@ip-10-205-227-133 ~]# prereq-checks-master/prereq-check.sh
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
DateTime:      Wed Nov 29 16:06:11 NZDT 2017
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

