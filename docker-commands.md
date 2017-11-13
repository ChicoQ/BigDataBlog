

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

