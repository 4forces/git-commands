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

* [Reference 1: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org)

* [Reference 2: Edureka](https://www.edureka.co/blog/docker-commands/#rmi)

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

E.g. 2: `CMD  ["command","param1"]`:`CMD  ["sleep","5"]` - In JSON  format

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
`docker run --entrypoint sleep2.0 ubuntu-sleeper 10`: The final command at startup will be `sleep2.0 10`

* [Reference: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org)