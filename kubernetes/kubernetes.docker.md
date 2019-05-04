# Kubernetes Udacity Course Notes

## Building the Containers with Docker

### Create a Project
* A GCP project is required for this course. You can use an existing GCP project or create a new one.


### Enable Compute Engine and Container Engine APIs
* In order to create the cloud resources required by this workshop, you will need to enable the following APIs using the Google API Console:
    * Compute Engine API
    * Container Engine API - > Kubernetes Engine API

### Enable and explore Cloud Shell
* Google Cloud Shell provides you with command-line access to computing resources hosted on Google Cloud Platform and is available now in the Google Cloud Platform Console

### Configure Your Cloud Shell Environment

```
gcloud compute zones list
gcloud config set compute/zone us-central1-c
```

### Download Go:
* Note: Cloud Shell comes with an installed Go, but it's not the most recent version, so you should perform the steps below to install the latest Go and set GOPATH.

```
wget https://storage.googleapis.com/golang/go1.6.2.linux-amd64.tar.gz
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.6.2.linux-amd64.tar.gz
echo "export GOPATH=~/go" >> ~/.bashrc
source ~/.bashrc
```
* Get the code
```
mkdir -p $GOPATH/src/github.com/udacity
cd $GOPATH/src/github.com/udacity
git clone https://github.com/udacity/ud615
```

* Get ready
```
cd ud615/app
```

## Docker
### Installing images with docker
```
sudo docker images
sudo docker pull nginx:1.10.0
```

### Running images with docker
```
sudo docker run -d nginx:1.10.0
sudo docker ps
sudo docker run -d nginx:1.9.3
sudo ps aux | grep nginx
```

### Talking to docker instances

```
sudo docker ps
sudo docker inspect c555faba3ab6        
curl http://172.18.0.3
```

```
sudo docker stop c555faba3ab6 ac8835266426
sudo docker rm c555faba3ab6 ac8835266426
```

### Creating your own image overview
* Dockerfile (File)
```
FROM alpine:3.1
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>
ADD hello /usr/bin/hello
ENTRYPOINT ["hello"]
```

### Create an Image

* Build a static binary of the monolith app
```
cd ud615/app/monolith
go get -u
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
```

* Create a container for the app
```
cat Dockerfile
```
```
FROM alpine:3.1
MAINTAINER Kelsey Hightower <kelsey.hightower@gmail.com>
ADD monolith /usr/bin/monolith
ENTRYPOINT ["monolith"]
```
* Build the app container
```
sudo docker build -t monolith:1.0.0 .
```
* List the monolith image
```
sudo docker images monolith:1.0.0
```
* Run the monolith container and get it's IP
```
sudo docker run -d monolith:1.0.0
sudo docker inspect <container name or cid>
```
* Test the container
```
curl 172.18.0.2
```

### Create docker images for the remaining microservices - auth and hello.
Repeat the steps you took for monolith.

* Build the auth app
```
cd $GOPATH/src/github.com/udacity/ud615/app
cd auth
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
sudo docker build -t auth:1.0.0 .
CID2=$(sudo docker run -d auth:1.0.0)
```

* Build the hello app
```
cd $GOPATH/src/github.com/udacity/ud615/app
cd hello
go build --tags netgo --ldflags '-extldflags "-lm -lstdc++ -static"'
sudo docker build -t hello:1.0.0 .
CID3=$(sudo docker run -d hello:1.0.0)
```

* See the running containers
```
sudo docker ps -a
```

### Push images

* See all images
```
sudo docker images
```

* Docker tag command help
```
docker tag -h
```

* Add your own tag
```
sudo docker tag monolith:1.0.0 <your username>/monolith:1.0.0
```

* For example (you can rename too!)
```
sudo docker tag monolith:1.0.0 udacity/example-monolith:1.0.0
```

* Create account on Dockerhub
To be able to push images to Dockerhub you need to create an account there - https://hub.docker.com/register/

* Login and use the docker push command
```
sudo docker login
sudo docker push udacity/example-monolith:1.0.0
```

* Repeat for all images you created - monolith, auth and hello!