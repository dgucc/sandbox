# Docker

## Install docker

`$ sudo apt-get install docker docker.io`

move docker folder :

```
$ sudo service docker stop
$ nano /etc/docker/daemon.json
{
    "data-root": "/home/jinx/docker"
}
$ sudo rsync -aP /var/lib/docker/ ~/docker
$ sudo mv /var/lib/docker /var/lib/docker.bak
$ sudo service docker start
$ sudo docker images
$ sudo rm -rf /var/lib/docker.old
```

Get rid of sudo (optional):

```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
$ docker run --rm hello-world
```

## Re-install docker CE

```
$ dpkg --list | grep docker 
$ sudo apt-get remove docker docker-engine docker.io containerd runc 

$ sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common 
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
$ sudo apt-key fingerprint 0EBFCD88 
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable" 
$ sudo apt-get update 
$ sudo apt-get install docker-ce docker-ce-cli containerd.io 
$ sudo docker run hello-world 

```

---

Disable Docker services

``` 
$ sudo systemctl disable docker.service   
$ sudo systemctl disable docker.socket 
```

Start it manually

`$ sudo service docker start`  
`$ sudo docker run --rm hello-world`  
`$ sudo docker ps -a`  
`$ sudo docker stop hello-world`  

---
## SQL Server 2017

[How to run SQL Server in a Docker container](https://blog.logrocket.com/docker-sql-server/)

```
$ docker run -itd --rm --name='sqlserver' -e 'ACCEPT_EULA=Y' -e   'SA_PASSWORD=SA_password' -p 1433:1433 -v $HOME/workspace/docker/mssql/data:/var/opt/mssql/data  mcr.microsoft.com/mssql/server:2017-latest
```

debug :

`$ docker exec -t sql1 cat /var/opt/mssql/log/errorlog | grep connection`

connecto to server :

`$ docker exec -it sql1 "bash"` 

```
$ /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P SA_password` 
1> select name from sys.databases 
2> go 
```
---

## DB2

Catalog database remote docker :  
```
db2 => CATALOG TCPIP NODE HOME REMOTE 192.168.1.11 SERVER 50000
db2 => CATALOG DATABASE LOCALDB2 AS HOMEDEV AT NODE HOME

db2 => UNCATALOG DATABASE HOMEDEV
db2 => UNCATALOG NODE home
```
Export with CALL ADMIN_CMD (on server side) :  
`CALL ADMIN_CMD('export to ~/data/export.csv of del modified by coldel, SELECT COL1,COL2 FROM MYSCHEMA.TABLE1');`

---


## Neo4j
https://ajstorm.medium.com/installing-db2-on-your-coffee-break-5be1d811b052

```
$ sudo docker run -d --rm \  
--publish=7474:7474 --publish=7687:7687 \  
--volume=$HOME/docker/neo4j/data:/data \  
--env=NEO4J_AUTH=none \
neo4j
```

http://localhost:7474/

---

## disable docker 
$ sudo systemctl disable docker.service   
$ sudo systemctl disable docker.socket  

---

## Last update : Errors

Errors were encountered while processing:  

```
 docker-ce  
E: Sub-process /usr/bin/dpkg returned an error code (1)  
A package failed to install.  Trying to recover:  
Setting up docker-ce (5:20.10.16~3-0~ubuntu-bionic) ...  
Job for docker.service failed because the control process exited with error code.  
See "systemctl status docker.service" and "journalctl -xe" for details.  
invoke-rc.d: initscript docker, action "restart" failed.  
● docker.service - Docker Application Container Engine  
	Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)  
	Active: activating (auto-restart) (Result: exit-code) since Sat 2022-05-14 10:51:20 CEST; 7ms ago  
TriggeredBy: ● docker.socket  
	Docs: https://docs.docker.com  
	Process: 10968 ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock (code=exited, status=1/FAILURE)  
	Main PID: 10968 (code=exited, status=1/FAILURE)  
dpkg: error processing package docker-ce (--configure):  
 installed docker-ce package post-installation script subprocess returned error exit status 1  
Errors were encountered while processing:  
 docker-ce  
```
