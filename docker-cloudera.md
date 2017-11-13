

##

[root@kafka ~]# /usr/share/cmf/schema/scm_prepare_database.sh mysql cm cm password 
JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/java/jdk1.7.0_67-cloudera/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
log4j:ERROR Could not find value for key log4j.appender.A
log4j:ERROR Could not instantiate appender named "A".
[2017-11-12 23:27:07,030] INFO     0[main] - com.cloudera.enterprise.dbutil.DbCommandExecutor.testDbConnection(DbCommandExecutor.java) - Successfully connected to database.
All done, your SCM database is configured correctly!



## history

yum install -y vim

vim /etc/sysconfig/network

[root@kafka ~]# cat /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=kafka.com


hostname
kafka.com


vim /etc/hosts

[root@kafka ~]# cat /etc/hosts
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
ff00::0	ip6-mcastprefix
ff02::1	ip6-allnodes
ff02::2	ip6-allrouters
172.17.0.2	kafka.com kafka


reboot
reboot -f
shutdown -r now

setenforce 0
vim /etc/selinux/config

service iptables status

yum repolist

## httpd

yum -y install httpd
chkconfig --add httpd
chkconfig httpd on
chkconfig --list | grep httpd

service httpd start

service iptables stop
service iptables status

## ntpd

yum -y install ntp
chkconfig --add ntpd
chkconfig ntpd on
vim /etc/ntp.conf 


```
[root@kafka ~]# cat /etc/ntp.conf 
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
restrict -6 ::1

# Hosts on local network are less restricted.
#restrict 192.168.1.0 mask 255.255.255.0 nomodify notrap

# Use public servers from the pool.ntp.org project.
# Please consider joining the pool (http://www.pool.ntp.org/join.html).

#server 0.centos.pool.ntp.org iburst
#server 1.centos.pool.ntp.org iburst
#server 2.centos.pool.ntp.org iburst
#server 3.centos.pool.ntp.org iburst

server	127.127.1.0
fudge	127.127.1.0	stratum 10

#broadcast 192.168.1.255 autokey	# broadcast server
#broadcastclient			# broadcast client
#broadcast 224.0.1.1 autokey		# multicast server
#multicastclient 224.0.1.1		# multicast client
#manycastserver 239.255.254.254		# manycast server
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
```

service ntpd restart

ntpq -p

     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
*LOCAL(0)        .LOCL.          10 l   15   64  377    0.000    0.000   0.000


## MySQL

yum -y install mysql mysql-server 
chkconfig --add mysqld
chkconfig mysqld on
service mysqld start

mysql_secure_installation

mysql -uroot -p


mkdir -p /usr/share/java

yum -y install wget

wget http://central.maven.org/maven2/mysql/mysql-connector-java/5.1.34/mysql-connector-java-5.1.34.jar

ln -s mysql-connector-java-5.1.34.jar mysql-connector-java.jar 

## repo

[root@kafka ~]# cat /etc/yum.repos.d/cm.repo 
[cmrepo]
name=CM5.12.1
baseurl=http://kafka.com/cm5.12.1
gpgcheck=false
enabled=true


cd /var/www/html/
mkdir -p cm5.12.1

cd cm5.12.1/
wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/cloudera-manager-agent-5.12.1-1.cm5121.p0.6.el6.x86_64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/cloudera-manager-daemons-5.12.1-1.cm5121.p0.6.el6.x86_64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/cloudera-manager-server-5.12.1-1.cm5121.p0.6.el6.x86_64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/cloudera-manager-server-db-2-5.12.1-1.cm5121.p0.6.el6.x86_64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/enterprise-debuginfo-5.12.1-1.cm5121.p0.6.el6.x86_64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/jdk-6u31-linux-amd64.rpm

wget http://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/RPMS/x86_64/oracle-j2sdk1.7-1.7.0+update67-1.x86_64.rpm


yum -y install createrepo
createrepo .

vim /etc/yum.repos.d/cm.repo

yum repolist
yum -y install oracle-j2sdk1.7-1.7.0+update67-1

cd ..
mkdir -p cdh5.12.1
cd cdh5.12.1/

wget http://archive.cloudera.com/cdh5/parcels/5.12.1/CDH-5.12.1-1.cdh5.12.1.p0.3-el6.parcel

wget http://archive.cloudera.com/cdh5/parcels/5.12.1/CDH-5.12.1-1.cdh5.12.1.p0.3-el6.parcel.sha1

wget http://archive.cloudera.com/cdh5/parcels/5.12.1/manifest.json

cd ~
yum -y install cloudera-manager-server
/usr/share/cmf/schema/scm_prepare_database.sh mysql cm cm password
service cloudera-scm-server start
netstat -nlpt | grep 7180
service cloudera-scm-server status
netstat -nlp | grep 7180

service sshd status

yum install -y openssh-server

service sshd start

passwd

service sshd restart

service cloudera-scm-server restart
netstat -nlp | grep 7180
service cloudera-scm-server status

scp
yum install -y openssh-clients

##

[root@kafka ~]# cat ~/.mysql_history

create database metastore default character set utf8;
create user 'hive'@'%' identified by 'password';
grant all privileges on metastore.* to 'hive'@'%';
flush privileges;

create database cm default character set utf8;
create user 'cm'@'%' identified by 'password';
grant all privileges on cm.* to 'cm'@'%';
flush privileges;

create database am default character set utf8;
create user 'am'@'%' identified by 'password';
grant all privileges on 'am'@'%';
grant all privileges on am.* to 'am'@'%';
flush privileges;

create database rm default character set utf8;
create user 'rm'@'%' identified by 'password';
grant all privileges on rm.* to 'rm'@'%';
flush privileges;

create database hue default character set utf8;
create user 'hue'@'%' identified by 'password';
grant all privileges on hue.* to 'hue'@'%';
flush privileges;

create database oozie default character set utf8;
create user 'oozie'@'%' identified by 'password';
grant all privileges on oozie.* to 'oozie'@'%';
flush privileges;

##

[root@kafka ~]# cat /etc/yum.repos.d/cloudera-manager.repo 
[cloudera-manager]
name = Cloudera Manager, Version 5.12.1
baseurl = https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.12.1/
gpgkey = https://archive.cloudera.com/redhat/cdh/RPM-GPG-KEY-cloudera
gpgcheck = 1


## ntpd error

[root@kafka /]# service ntpd start
Starting ntpd: ntpd: error while loading shared libraries: libm.so.6: cannot open shared object file: Permission denied
                                                           [FAILED]

[root@kafka /]# service ntpd start
Starting ntpd: ntpd: error while loading shared libraries: libm.so.6: cannot open shared object file: Permission denied
                                                           [FAILED]
[root@kafka /]# 
[root@kafka /]# ldd /usr/sbin/ntpd
	linux-vdso.so.1 =>  (0x00007ffd8f3de000)
	libm.so.6 => /lib64/libm.so.6 (0x00007fe1e368b000)
	libcrypto.so.10 => /usr/lib64/libcrypto.so.10 (0x00007fe1e32a6000)
	librt.so.1 => /lib64/librt.so.1 (0x00007fe1e309e000)
	libcap.so.2 => /lib64/libcap.so.2 (0x00007fe1e2e9a000)
	libc.so.6 => /lib64/libc.so.6 (0x00007fe1e2b06000)
	libdl.so.2 => /lib64/libdl.so.2 (0x00007fe1e2902000)
	libz.so.1 => /lib64/libz.so.1 (0x00007fe1e26ec000)
	libpthread.so.0 => /lib64/libpthread.so.0 (0x00007fe1e24cf000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fe1e3c1f000)
	libattr.so.1 => /lib64/libattr.so.1 (0x00007fe1e22ca000)
[root@kafka /]# 
[root@kafka /]# ll /lib64/libm.so.6 
lrwxrwxrwx 1 root root 12 Aug  2 05:15 /lib64/libm.so.6 -> libm-2.12.so
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# cat /etc/sysconfig/selinux
cat: /etc/sysconfig/selinux: No such file or directory
[root@kafka /]# 

##

[root@kafka /]# setenforce 0
setenforce: SELinux is disabled

[root@kafka /]# reboot
shutdown: Unable to shutdown system


[root@kafka /]# cat /proc/sys/vm/swappiness 
60
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# sysctl vm.swappiness=10
vm.swappiness = 10
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# cat /proc/sys/vm/swappiness 
10
[root@kafka /]# 



