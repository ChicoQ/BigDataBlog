## java.rmi.server.ExportException: Port already in use: 9999

root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# 
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.72:9092 --from-beginning --topic
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# 
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.72:2181 --from-beginning --topic
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# 
root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --zookeeper 192.168.1.72:2181 --from-beginning --topic
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)

root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# netstat -nlp | grep 9999
tcp        0      0 0.0.0.0:9999            0.0.0.0:*               LISTEN      30107/java  

## vim bin/kafka-server-start.sh

```
export KAFKA_JMX_OPTS="-Dcom.sun.management.jmxremote=true -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Djava.rmi.server.hostname=chico-gtx -Djava.net.preferIPv4Stack=true"
export JMX_PORT=9999
```

root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --zookeeper 192.168.1.72:2181 --from-beginning --topic test
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)

root@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1# bin/kafka-console-consumer.sh --bootstrap-server 192.168.1.72:2181 --from-beginning --topic test
Error: Exception thrown by the agent : java.rmi.server.ExportException: Port already in use: 9999; nested exception is: 
	java.net.BindException: Address already in use (Bind failed)

## Setup Kafka

chico@chico-gtx:~$ java -version
openjdk version "1.8.0_151"
OpenJDK Runtime Environment (build 1.8.0_151-8u151-b12-0ubuntu0.16.04.2-b12)
OpenJDK 64-Bit Server VM (build 25.151-b12, mixed mode)

chico@chico-gtx:~$ sudo apt-get install zookeeperd

chico@chico-gtx:~$ sudo netstat -ant | grep 2181
tcp6       0      0 :::2181                 :::*                    LISTEN    

chico@chico-gtx:~$ cd Downloads/
chico@chico-gtx:/media/hdd1$ sudo tar -xvf ~/Downloads/kafka_2.11-0.10.1.1.tgz 

chico@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1$ bin/kafka-topics.sh --create --zookeeper 192.168.1.72:2181 --replication-factor 1 --partitions 1 --topic test

chico@chico-gtx:/media/hdd1/kafka_2.11-0.10.1.1$ bin/kafka-console-producer.sh --broker-list 192.168.1.72:9092 --topic test

## jmxtrans

https://github.com/jmxtrans/jmxtrans/wiki/Installation
chico@chico-gtx:/media/hdd1$ sudo dpkg -i jmxtrans-268.deb 

chico@chico-gtx:/media/hdd1$ ll /usr/share/jmxtrans/
total 32
drwxr-xr-x   6 jmxtrans jmxtrans  4096 Nov 27 11:04 ./
drwxr-xr-x 322 root     root     12288 Nov 27 11:04 ../
drwxr-xr-x   2 jmxtrans jmxtrans  4096 Nov 27 11:04 bin/
drwxr-xr-x   5 jmxtrans jmxtrans  4096 Nov 27 11:04 doc/
drwxr-xr-x   2 jmxtrans jmxtrans  4096 Nov 27 11:04 lib/
lrwxrwxrwx   1 root     root        17 Nov 27 11:04 logs -> /var/log/jmxtrans/
drwxr-xr-x   2 jmxtrans jmxtrans  4096 Nov 27 11:04 tools/

chico@chico-gtx:/media/hdd1$ ll /etc/default/jmxtrans 
-rw-r--r-- 1 root root 287 Nov 27 11:04 /etc/default/jmxtrans

chico@chico-gtx:/media/hdd1$ sudo /etc/init.d/jmxtrans status

chico@chico-gtx:/media/hdd1$ ll /var/lib/jmxtrans/
total 8
drwxr-xr-x  2 jmxtrans jmxtrans 4096 Nov 27 11:04 ./
drwxr-xr-x 78 root     root     4096 Nov 27 11:04 ../

chico@chico-gtx:/var/lib/jmxtrans$ ll
total 48
drwxr-xr-x  2 jmxtrans jmxtrans  4096 Nov 27 16:11 ./
drwxr-xr-x 78 root     root      4096 Nov 27 11:04 ../
-rw-r--r--  1 root     root     20190 Nov 27 16:09 ApacheKafkaConsumerMetrics.json
-rw-rw-r--  1 root     root      8444 Nov 27 11:31 base_172.168.1.72.json
-rw-r--r--  1 root     root      1329 Nov 27 16:11 consumer.json
-rw-rw-r--  1 root     root      2025 Nov 27 11:33 falcon_monitor_us_72.json

## influx

chico@chico-gtx:~$ curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -

chico@chico-gtx:~$ source /etc/lsb-release 
chico@chico-gtx:~$ echo "deb https://repos.influxdata.com/${DISTRIB_ID,,} ${DISTRIB_CODENAME} stable" | sudo tee /etc/apt/sources.list.d/influxdb.list
deb https://repos.influxdata.com/ubuntu xenial stable

chico@chico-gtx:~$ sudo apt-get update && sudo apt-get install influxdb

chico@chico-gtx:~$ sudo service influxdb start

chico@chico-gtx:~$ influx
Connected to http://localhost:8086 version 1.3.7
InfluxDB shell version: 1.3.7
> create user "root" with password '123456' with all privileges
> create database jmxDB
> show databases
name: databases
name
----
_internal
jmxDB
> quit

chico@chico-gtx:~$ sudo netstat -nlp | grep 8086
[sudo] password for chico: 
tcp6       0      0 :::8086                 :::*                    LISTEN      7153/influxd   

## grafana

chico@chico-gtx:/media/hdd1$ sudo vim /etc/apt/sources.list
chico@chico-gtx:/media/hdd1$ curl https://packagecloud.io/gpg.key | sudo apt-key add -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  3889  100  3889    0     0   2945      0  0:00:01  0:00:01 --:--:--  2943
OK
chico@chico-gtx:/media/hdd1$ sudo apt-get update

chico@chico-gtx:~$ sudo apt-get install grafana

chico@chico-gtx:/media/hdd1$ sudo service grafana-server start

