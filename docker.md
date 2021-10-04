# Docker

## Common Commands

`docker –version`: Shows docker version. Can also check if docker is installed

`docker pull [image]`: Download image without running it

`docker exec [container_Name] [command]`: Executes command on running container

`docker ps`: Lists running container with basic information

`docker ps -a`: Lists all running and previously stopped containers

`docker stop [container_name/Id]`: Stops the docker container

docker kill

docker commit

docker login

docker push

`docker images`: Shows all installed docker images and related information

`docker rm [container_name/Id]`: Remove a stopped or exited permanently from disk

`docker rmi [image]`: Remove a downloaded image. Delete all dependent containers first.

`docker history [image]`: View details of the image layers, like 'created by' and 'size'.

`docker inspect [container_name/id]`: view details of container like "id", "state", "mounts", etc

* All contents from [reference 1: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org) and [reference 2: Edureka](https://www.edureka.co/blog/docker-commands/#rmi)

## docker run

`docker run [docker_image]`: run a container from an image

*A container only exists as long as the process inside is alive. If the web service inside the container is stopped or crashed, the container exits.*

`docker run ubuntu sleep 5`: Run ubuntu with a sleep command for 5s. 

*The container running the Ubuntu image will be alive as long as the sleep command is active. The container will close after sleep command exits* 

`docker run [image]:4.0`: ‘4.0’ refers to tag (version). Default is ‘latest’ tag

`docker run -d [image]`: Run container in detached (background) mode. Container will not take over console, leaving it for user inputs. 

`docker attach [container_id]`: Runs the container in attached mode (foreground) in terminal. 

`docker run -i`: interactive mode, allows user interaction

`docker run -t`: with pseudo terminal (from/on the container)

`docker run -p 80:5000 [image]`: map port 5000 on container to port 80 (on docker/local host level).

**Note: Each docker container (within the docker host) has their own IP and port. The docker host also has its own IP. We are mapping the docker container port to a port at docker host leve for external users to access the docker container.*
*E.g. `.0.0.0:3456->3456/tcp, 0.0.0.0:38080->80/tcp` - ports published on hosts -> ports exposed*


`docker run -v [/local_data_dir_path]:[/container_data_dir_path] [image]`: maps container 'data folder' to local folder. This is for data retention as data folder is destroyed when container is deleted. 

## Environment Variables

An example of setting the env variable in the .py and running it from docker:
```python
# sample code of a webapp showing change of color

import os
from flask import Flask

color = os.environ.get('APP_COLOR') # previously color = "red" 
```
```
// run following commands (in console?) to apply color = blue
export APP_COLOR=blue; python app.py
```
`docker run -e APP_COLOR=blue [image]`: apply the env variable APP_COLOR=blue and run the (above) image

## Images

**Steps**
1. List down the steps/instructions to be done first
2. Create a Dockerfile with following the steps listed, e.g.
```
//DockerFile
// [INSTRUCTION] [argument]

FROM Ubuntu // defines base OS for container

Run apt-get update // runs commands on the image, for e.g apt-get update, install python, etc
RUN apt-get install python 

RUN pip install flask
RUN pip install flask-mysql

COPY . /opt/source-code // copy everything to the folder in the image 

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run // specifies a command to run when the image is build in the container
```

3. Run `docker build Dockerfile -t [my/custom-app/folder/image-name]` to create image locally on system. The steps involved in building the various layers for the image, for e.g. getting core image, installing python dependencies, copying app.py etc will be shown in console. Each step builds a layer on top of the previous layer.
*Note: docker build can be runned on individual step id to avoid restart from beginning, for eg if we update source code for our app*

4. Run `docker push [my/custom-app/folder/image-name]`

## CMD vs ENTRYPOINT

**Script for specific commands to run the CMD file**

```
// ubuntu-sleeper file
FROM Ubuntu

CMD sleep 5 // runs the sleep command for 5 seconds
```

To build the above CMD file:
`docker build -t ubuntu-sleeper`

To run:
`docker run ubuntu-sleeper`

**Formats to type CMD**

E.g. 1: `CMD command param1`:`CMD sleep 5` - standard format

E.g. 2: `CMD  ["command","param1"]`:`CMD  ["sleep","5"]` - In JSON format

**CMD vs ENTRYPOINT**
```
// ubuntu-sleeper file with 'empty' sleep command
FROM Ubuntu

ENTRYPOINT ["sleep"] // command to run at start
```

Run with variable: `docker run ubuntu-sleeper 10` - Command at Startup: sleep 10 
('10' is user input, 'sleep' from ENTRYPOINT)

**Specify a default CMD with optional ENTRYPOINT**
```
// ubuntu-sleeper file with 'empty' sleep command and default CMD argument
FROM Ubuntu

ENTRYPOINT ["sleep"] // command to run at start

CMD ["5"] // 5 will be default argument if no user input. Must be in JSON format
```

**Overwrite the original sleep CMD with newly created sleep2.0 ENTRYPOINT CMD**
`docker run --entrypoint sleep2.0 ubuntu-sleeper 10`: 
The final command at startup will be `sleep2.0 10`

## Networking

1. Bridge Network

   `docker run Ubuntu`: Creates an internal network connecting (bridging) all the containers within the host

2. Use Host Network

   `docker run Ubuntu --network=host`: All containers interface with external world via the host port (5000). Therefore only 1 container can be used in 1 host.

3. No network

   `docker run Ubuntu --network=none`: Disables network connection for the container

**User-define Networks**

```
\\ 1. Defining which containers share which network bridge

docker network create \
    --driver bridge    \    \\specify driver (bridge/host/overlay/null)
    --subnet 182.18.0.0/16  \\specify subnet
    --gateway 182.18.0.1    \\specify gateway
    custom-isolated-network \\custom network name
```
```
\\ 2. To list all networks, run the following command

docker network ls
```
```
\\ 3. To list network settings including IPAddress and MacAddress, type

docker inspect [Id/Name]
```
**Embedded DNS**
* Docker runs a DNS Server internally in the host at IP 127.0.0.1
* Use the container name to to connect containters (for e.g web container to mysql DB container), NOT the IP which will be dynamic every session. 

**Examples**
```
\\ 1. set up a mysql db container with the following params

docker run -d \                      \\run in -d detached, -e env var input
-e MYSQL_ROOT_PASSWORD=db_pass123 \  \\set env var
--name mysql-db \                    \\set container name
--network wp-mysql-network \         \\set network name
mysql:5.6                            \\set image to use (with tag)
```
```
\\ 2. Deploy a web app with the following params. Expose the port to 38080 on host

docker run \
--network=wp-mysql-network \        \\attach to created network
-e DB_Host=mysql-db \               \\set env var
-e DB_Password=db_pass123 \         \\set env var
-p 38080:8080 \                     \\expose port to 38080 on the host
--name webapp \                     \\set container name
--link mysql-db:mysql-db \          \\link this webapp with mysql container
-d kodekloud/simple-webapp-mysql    \\set image to use
```

## Docker Storage

* Image Layer(Read Only) - Like ROM
* Container layer (Read/Write) - Like HDD

### Volumes

1. `docker volume create data_volume`: Creates a volume named `data_volume` in default docker host volumes directory: `/var/lib/docker/volumes` 

1. `docker run -v data_volume:/var/lib/mysql mysql`: Mount `data_volume` to (:) `/var/lib/mysql` in the newly created `mysql` cotainer
   - `/var/lib/mysql` is the default location where mysql stores data
   - When the new container from `mysql` image is created, the newly created volume `data_volume` is mounted to this new container's folder
   - Even if the container is destroyed, the data is still retained in the volume
   - aka Volume Mounting

1. `docker run -v /data/mysql:/var/lib/mysql mysql`: Mount volume in custom directory in host `/data/mysql` to (:) `/var/lib/mysql` in the newly created `mysql` cotainer 
   - aka Bind Mounting, where a directory from any location on the docker host (not volume folder) is mounted to a new container

1. `docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql`: New, more verbose way of writing the above command
   - source: denote location on host
   - target: denote location on container

1. Docker uses storage drivers ( AUF, ZFS, Device Mapper, etc) for mountings. Storage drivers are OS dependent.

## Docker Compose

1. `docker-compose up docker-compose.yml`: Used to combine multiple commands to set up apps, db, etc instead of running multi single line commands

1.  Example of `docker-compose.yml` file:
```docker
services:
  redis:                            //name of container 1
    image: redis:alpine             //image to use
  clickcounter:                     //name of container 2
    image: kodekloud/click-counter  
    ports:
    - 8085:5000                     //hostport:containerport
version: '3.0'                      //docker-compose.yml version
```

3. Type `docker-compose -up docker-compose.yml` to run the `yml` file

## Docker Registry

1. `docker login private-registry.io`: Login to registry

2. `docker run private-registry.io/apps/internalapp`: Run private registry as part of image name

3. `docker run -d -p 5000:5000 --name registry registry:2`: Run custom image `registry` on `port:5000`.

   *Note - docker registry is available as a Docker image. Image name:`registry`, exposes API on `port:5000`*

4. `docker image tag my-image localhost:5000/my-image`: tag `my-image` with private registry url `localhost:5000/my-image`. 

   *Note that since the image is running on the same docker host, we can use `localhost` on port `5000` followed by image name `my-image`*

5. `docker push localhost:5000/my-image`: Push image to private registry

6. `doker pull localhost:5000/my-image` Pull image from anywhere on the same network/same host using

6. `docker pull 192.168.56.100:5000/my-image` Pull image from the IP or domain name (if user accessing from another host in the network environment)

## Docker Engine

Docker engine consists of:
   1. Docker CLI 
      - Docker CLI can be on a different machine
         - E.g. 1. To connect to remote docker engine at address `remote-docker-engine` and port `2375`, type `docker -H=remote-docker-engine:2375`
         - E.g. 2. To a container based on `nginx` on a remote Docker host, type `docker -H=10.123.2.1:2375 run nginx`
   2. REST API
   3. Docker daemon

### Resource management for Docker containers

Docker uses *cgroups* to manage hardware resources

   - `docker run --cpus=.5 ubuntu`: Use maximum 50% CPU

   - `docker run --memory=100m ubuntu`: Use maximum 100 MB memory

## Container Orchestration

- Tools/scripts to help hosts docker containers

- `docker service create --replicas=100 nodejs`: Docker swarm cmd to create 100 instances fo `nodejs`

- instances scaling according to demand

- additional host to super user load

- networking and storage management, etc between different containers in different hosts

- Examples: docker swarm, kubernetes, Mesos
---

* All contents from [reference: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org)