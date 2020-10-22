# Notes from CD with Docker and Jenkins

### Chapter 2

```cmd
docker search mongo
```

* Run a container and connect it to its command line
```cmd
docker run -i -t ubuntu:18.04 /bin/bash
```

* Install git
```bash
apt-get update
apt-get install -y git
```

* Show what has changed
```cmd
docker diff ad8f2b034359
```

* Commit the container to the image
```cmd
docker commit ad8f2b034359 ubuntu_with_git
```

* Run our image
```cmd
docker container run -i -t ubuntu_with_git /bin/bash
```
* Add JDK
```bash
apt-get install -y openjdk-8-jdk
exit
```

```cmd
docker commit f087a904a189 ubuntu_with_git_and_jdk
```


### Dockerfile
```Dockerfile
FROM ubuntu:18.04
RUN apt-get update && \
    apt-get install -y python
ENV NOMBRE JCC
COPY hello.py .
ENTRYPOINT ["python","hello.py"]
```

* Create the image
```cmd
docker image build -t ubuntu_with_python .
```

* Run the image (Reading environment variables from Dockerfile)
```cmd
docker container run ubuntu_with_python
```
Output: HELLO WORLD from Python JCC!

* Run the image with Environment variables(Reading environment variables from command line)
```cmd
docker container run -e NOMBRE=DEMO123 ubuntu_with_python
```
Output: HELLO WORLD from Python DEMO123!


### Networking
* Port Mapping: -p, --publish <hostport>:<container_port>
```cmd
docker container run -d -p 8080:8080 tomcat
```

* Query the port info
```cmd
docker port e17674bba528
```

### Docker volumes
* -v <host_path>:<container_path>
```cmd
docker run -i -t -v "C:\bd\var\lib\mysql-files":/var/lib/mysql-files/ mysql:5.7.27 
```

```Dockerfile
VOLUME /var/lib/mysql-files/ mysql:5.7.27
```

### Naming containers (--name)
```cmd
docker container run -d --name mytomcat tomcat
```

### Tagging images (-t)
```
docker image build -t hello_world_python .
```


