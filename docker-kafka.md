1  sudo apt-get update
2  sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common
3  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
4  sudo apt-key fingerprint 0EBFCD88
5  sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
6     $(lsb_release -cs) \
7     stable"
8  sudo apt-get update
9  sudo apt-get install docker-ce
10  sudo docker run hello-world
11  docker ps
12  sudo docker ps
13  sudo docker run hello-world
14  sudo docker pull cloudera/quickstart:latest
15  docker images
16  sudo docker images
17  docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180  4239cd2958c6 /usr/bin/docker-quickstart
18  sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180  4239cd2958c6 /usr/bin/docker-quickstart
19  sudo docker list
20  sudo docker ps
21  sudo docker help
22  sudo docker port
23  cd Downloads/
24  ll
25  tar xvzf cloudera-quickstart-vm-5.12.0-0-beta-docker.tar.gz
26  ll
27  ll cloudera-quickstart-vm-5.12.0-0-beta-docker
28  sudo docker import - cloudera/quickstart:latest < cloudera-quickstart-vm-5.12.0-0-beta-docker/cloudera-quickstart-vm-5.12.0-0-beta-docker.tar
29  sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180 2a8467e137fe /usr/bin/docker-quickstart
30  top
31  sudo docker
32  sudo docker images
33  sudo docker ps
34  sudo docker inspect 1cd78f634747
35  ipconfig
36  ip add
37  date
38  timedatectl status | grep "Time zone"
39  timedatectl list-timezones
40  timedatectl list-timezones | grep Auckland
41  timedatectl
42  sudo docker images
43  sudo docker run
44  sudo docker network 2a8467e137fe
45  sudo docker network inspect 2a8467e137fe
46  sudo docker inspect 2a8467e137fe
47  ifconfig
48  sudo docker ps
49  sudo docker commit 7add1cc0def9 chicoqi/cdhKafka:version1
50  sudo docker commit 7add1cc0def9 chicoqi/cdh-kafka:version1
51  sudo docker images
52  sudo docker ps
53  sudo docker stop 7add1cc0def9
54  sudo docker ps
55  cd Downloads/
56  ll
57  history


chico@chico-HP:~/Downloads$ sudo docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
chicoqi/cdh-kafka     version1            4a98c46478d5        22 hours ago        7.71GB
cloudera/quickstart   latest              2a8467e137fe        23 hours ago        6.98GB
hello-world           latest              725dcfab7d63        6 days ago          1.84kB
cloudera/quickstart   <none>              4239cd2958c6        19 months ago       6.34GB
chico@chico-HP:~/Downloads$
chico@chico-HP:~/Downloads$
chico@chico-HP:~/Downloads$
chico@chico-HP:~/Downloads$ sudo docker run --hostname=quickstart.cloudera --privileged=true -t -i -p 8888:8888 -p 80:80 -p 7180:7180 4a98c46478d5 /usr/bin/docker-quickstart


## centos

Bad : The host's NTP service could not be located or did not respond to a request for the clock offset.

[root@quickstart yum.repos.d]# date
Fri Nov 10 21:40:50 NZDT 2017

[root@quickstart ~]# ll /etc/localtime
lrwxrwxrwx 1 root root 36 Nov  9 21:37 /etc/localtime -> /usr/share/zoneinfo/Pacific/Auckland


[root@quickstart ~]# service ntpd status
ntpd dead but pid file exists
[root@quickstart ~]#
[root@quickstart ~]#
[root@quickstart ~]# service ntpd restart
Shutting down ntpd:                                        [FAILED]
Starting ntpd:                                             [  OK  ]
