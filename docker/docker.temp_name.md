# Docker Notes

* Reference: https://git.github.com/bradtraversy/89fad226dc058a41b596d586022a9bd3

## Installation steps
* Download Docker for windows
* Install docker following the installation steps.
* In addition to Hyper-V, virtualization must be enabled. Check the Performance tab on the Task Manager:
* In the bios enable: Virtualization Technology


## First steps
* commands
```cmd
docker
docker version
docker info

```

* Create an run a container in foreground
```cmd
docker container run -it -p 80:80 nginx
```

* A batch of useful commands
```cmd
docker container list
docker container list -a
docker container stop 7d...
docker container rm 7d...
docker container list
docker images
docker image rm 2bc
docker images
docker ps
docker ps -a
```

* Pull an image
```cmd
docker pull nginx
docker images
```

* Create and run a container
```cmd
docker container run -d -p 8080:80 --name mynginx nginx

```
* Create and run one more container
```cmd
docker container run -d -p 8081:80 --name myapache httpd

```
* Create and run one more container
```cmd
docker container run -d -p 3306:3306 --name mysql --env MYSQL_ROOT_PASSWORD=123456 mysql
```

* Stop a container
```cmd
docker ps
docker container stop mysql
dokcer ps -a

```

* Force remove of a container
```cmd
docker container rm myapache -f
```
* Use exec to access the container by using bash
```cmd
docker container exec -it mynginx bash
```

* Remove all three containers
```cmd
docker container rm myapache mysql mynginx -f
```


### Create an image
* Create and run a container binded to a client directory 
```cmd
docker container run -d -p 8080:80 -v "c:\jcabelloc\workspace\docker\test":/usr/share/nginx/html --name nginx-website nginx
```
Then, we can create index.html on the windows path, and it will publish on the web site

* Create a Dockerfile on the windows path
```
FROM nginx:latest

WORKDIR /usr/share/nginx/html

COPY . .

```

* Build the image
```cmd
docker image build -t jcabelloc/nginx-website .
docker images

```

* Remove the container

```cmd
docker rm nginx-website -f
```

* Create and run a container based on our created image
```cmd
docker container run -d -p 8082:80 jcabelloc/nginx-website
docker ps

```

* Push that image on the docker hub repository
```cmd
docker push jcabelloc/nginx-website
```

* Inspect a container
```cmd
docker inspect nombre_container

```

* Run a container in the background -d (--detach)
```cmd
docker container run -d -t ubuntu:18.04
```

* Fetch the logs of a container
```cmd
docker logs e17674bba528
```

### Useful tips
* Connect as a root
```cmd
docker container exec -u root -it jenkins /bin/bash
```

```cmd
docker -H localhost:2375 info
docker -H localhost:2375 ps -a
```

* Connect from a container to a its windows docker engine: docker.for.win.localhost:2375