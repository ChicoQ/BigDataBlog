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



