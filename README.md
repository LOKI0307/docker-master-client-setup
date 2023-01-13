"# docker-master-client-setup" 
Agenda:- Here we will try to setup master client architecture for docker.

Pre-requisite:- 
> We should have 2 ubuntu 20.4 server with network connevctivity
> inbound rule should have port 2375 open for master server.

Steps:-
Step 1) Login into master server and setup hostname and hosts entry.

````
sudo su -
hostnamectl set-hostname docker-server
vi /etc/hosts
docker-server-ip docker-server
docker-client-ip docker-client
````

Step 2) Same step need to perform on client server.
````
sudo su -
hostnamectl set-hostname docker-server
vi /etc/hosts
docker-server-ip docker-server
docker-client-ip docker-client
````
Step 3) Come back to to master server and run below command to install docker.
````
apt-get update -y
apt-get install git apt-transport-https ca-certificates curl software-properties-common -y
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-get install docker-ce docker-compose -y
docker --version
````
Step 4) We will need to create a directory to store the Docker daemon configuration file.
````
mkdir -p /etc/systemd/system/docker.service.d
vi /etc/systemd/system/docker.service.d/options.conf
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -H unix:// -H tcp://0.0.0.0:2375
````
Step 5) Then we need to reload and restert docker service.
````
systemctl daemon-reload
systemctl restart docker
ps aux | grep dockerd
````
Step 6) Login into the client service need to perform same steps for installing docker on it.
````
apt-get update -y
apt-get install git apt-transport-https ca-certificates curl software-properties-common -y
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-get install docker-ce docker-compose -y
docker --version
````
Step 7) Now we need to add DOCKER_HOST variable to define the Docker daemon address on client server.
````
echo "export DOCKER_HOST=tcp://docker-server:2375" >> ~/.bashrc
source ~/.bashrc
````
Step 8) Perform testing by pulling image on master server and search same image on client node.
Master:-
````
docker pull ubuntu
````
Client:-
````
docker images
````

###########Thank you###############
# Please like and share this video
# This installation document is available at below git link
# https://github.com/LOKI0307/docker-master-client-setup
