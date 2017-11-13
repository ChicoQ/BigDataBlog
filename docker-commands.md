

chico@chico-HP:~ $ history

sudo apt-get update
sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) \
stable"

sudo apt-get update

sudo apt-get install docker-ce
sudo docker run hello-world

sudo docker ps
sudo docker run hello-world
sudo docker pull cloudera/quickstart:latest

sudo docker images

sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180  4239cd2958c6 /usr/bin/docker-quickstart

cd Downloads/
tar xvzf cloudera-quickstart-vm-5.12.0-0-beta-docker.tar.gz 
ll cloudera-quickstart-vm-5.12.0-0-beta-docker

sudo docker import - cloudera/quickstart:latest < cloudera-quickstart-vm-5.12.0-0-beta-docker/cloudera-quickstart-vm-5.12.0-0-beta-docker.tar 

sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180 2a8467e137fe /usr/bin/docker-quickstart
sudo docker ps

sudo docker inspect 1cd78f634747

date
timedatectl status | grep "Time zone"
timedatectl list-timezones
timedatectl list-timezones | grep Auckland
timedatectl

sudo docker run

sudo docker network 2a8467e137fe
sudo docker network inspect 2a8467e137fe
sudo docker inspect 2a8467e137fe

sudo docker commit 7add1cc0def9 chicoqi/cdh-kafka:version1

sudo docker ps
sudo docker stop 7add1cc0def9

sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180 4a98c46478d5 /usr/bin/docker-quickstart

sudo docker stop f7addc81a682

ssh root@172.17.0.2

sudo docker commit 2d76fd562f99 chicoqi/cdh-kafka:v1

ll /var/lib/docker

sudo docker login
sudo docker tag b0b47788f8b6 chico77/cdh-kafka
sudo docker push chico77/cdh-kafka

## timezone

[root@kafka /]# date
Mon Nov 13 05:42:26 UTC 2017
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# cp /etc/localtime /etc/localtime.old
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# rm /etc/localtime
rm: remove regular file `/etc/localtime'? y
[root@kafka /]# 
[root@kafka /]# ln -s /usr/share/zoneinfo/Pacific/Auckland /etc/localtime
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# date
Mon Nov 13 18:43:10 NZDT 2017
[root@kafka /]# 
[root@kafka /]# 
[root@kafka /]# ll /etc/localtime
lrwxrwxrwx 1 root root 36 Nov 13 18:43 /etc/localtime -> /usr/share/zoneinfo/Pacific/Auckland
[root@kafka /]# 

##

chico@chico-gtx:~$ sudo docker ps
[sudo] password for chico: 
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                                                                NAMES
e35a50481443        4ebb92c5f0e3        "/bin/bash"         About an hour ago   Up About an hour    0.0.0.0:80->80/tcp, 0.0.0.0:7180->7180/tcp, 0.0.0.0:8888->8888/tcp   elated_roentgen
chico@chico-gtx:~$ 
chico@chico-gtx:~$ 
chico@chico-gtx:~$ 
chico@chico-gtx:~$ 
chico@chico-gtx:~$ sudo docker commit e35a50481443 chicoqi/cdh-kafka:v3
sha256:5e5b20ff4c5ab6f7ae98162a2a06ecec3485b5a4abfac156103d512eca928ef9
chico@chico-gtx:~$ 
chico@chico-gtx:~$ 
chico@chico-gtx:~$ sudo docker images
REPOSITORY             TAG                           IMAGE ID            CREATED             SIZE
chicoqi/cdh-kafka      v3                            5e5b20ff4c5a        12 seconds ago      11.6GB
chicoqi/cdh-kafka      v2                            4ebb92c5f0e3        3 hours ago         5.73GB
chico77/cdh-kafka      latest                        b0b47788f8b6        6 hours ago         3.38GB
ubuntu                 latest                        14f60031763d        3 months ago        120MB
hello-world            latest                        48b5124b2768        10 months ago       1.84kB
cloudera/clusterdock   latest                        3e15a0e12577        14 months ago       463MB
cloudera/clusterdock   cdh580_cm581_secondary-node   31b6f9ea419e        15 months ago       4.64GB
cloudera/clusterdock   cdh580_cm581_primary-node     1e180df693af        15 months ago       4.5GB
chico@chico-gtx:~$ 
chico@chico-gtx:~$ 
