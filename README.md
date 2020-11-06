#Create a Bridge network
sudo docker network create --subnet=172.20.0.0/16 kumar-example-net
#Clone this repository with your linux machine
git clone https://github.com/kumarshreyansh/dns-project.git
#Run below mentioned commands
cd dns-project/
sudo docker build -t bind9 .
sudo docker run -d --rm --name=dns-server --net=kumar-example-net --ip=172.20.0.2 bind9
sudo docker exec -d dns-server /etc/init.d/bind9 start
sudo docker run -d --rm --name=www --net=kumar-example-net --ip=172.20.0.3 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"
sudo docker run -d --rm --name=sub2 --net=kumar-example-net --ip=172.20.0.4 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"
sudo docker exec -it www bash
#Run below mentioned commands inside conatiner for verification of dns server
apt-get update
apt-get install iputils-ping
ping sub2.kumar-example.com
ping www.kumar-example.com
