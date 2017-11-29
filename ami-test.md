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


