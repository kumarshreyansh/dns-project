#Create a Bridge network
1. sudo docker network create --subnet=172.20.0.0/16 kumar-example-net
2. git clone https://github.com/kumarshreyansh/dns-project.git #Clone this repository with your linux machine
#Run below mentioned commands
3. cd dns-project/
4. sudo docker build -t bind9 .
5. sudo docker run -d --rm --name=dns-server --net=kumar-example-net --ip=172.20.0.2 bind9
6. sudo docker exec -d dns-server /etc/init.d/bind9 start
7. sudo docker run -d --rm --name=www --net=kumar-example-net --ip=172.20.0.3 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"
8. sudo docker run -d --rm --name=sub2 --net=kumar-example-net --ip=172.20.0.4 --dns=172.20.0.2 ubuntu:bionic /bin/bash -c "while :; do sleep 10; done"
9. sudo docker exec -it www bash
#Run below mentioned commands inside conatiner for verification of dns server
10. apt-get update
11. apt-get install iputils-ping
12. ping sub2.kumar-example.com
13. ping www.kumar-example.com
