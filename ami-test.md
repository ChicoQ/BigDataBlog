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

## mysql driver

- download `mysql-connector-java-5.1.44.jar` from https://mvnrepository.com/artifact/mysql/mysql-connector-java/5.1.44
- copied above jar file into `/usr/share/java` and created a link
```
ln -s mysql-connector-java-5.1.44.jar mysql-connector-java.jar
```

## final result

```
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
 
```

