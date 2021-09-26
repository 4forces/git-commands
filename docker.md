# Docker Commands

## Common Commands

`docker –version`: Shows docker version. Can also check if docker is installed

`docker pull [image]`: Download image without running it

`docker run [docker_image]`: run a container from an image

    *A container only exists as long as the process inside is alive. If the web service inside the container is stopped or crashed, the container exits.*

`docker run ubuntu sleep 5`: Run ubuntu with a sleep command for 5s. 

*The container running the Ubuntu image will be alive as long as the sleep command is active. The container will close after sleep command exits* 

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

docker build

`docker inspect [container_name/id]`: view details of container like "id", "state", "mounts", etc

* [Reference 1: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org)

* [Reference 2: Edureka](https://www.edureka.co/blog/docker-commands/#rmi)

## More on docker run

`docker run [image]:4.0`: ‘4.0’ refers to tag (version). Default is ‘latest’ tag

`docker run -d [image]`: Run container in detached (background) mode. Container will not take over console, leaving it for user inputs. 

`docker attach [container_id]`: Runs the container in attached mode (foreground) in terminal. 

`docker run -i`: interactive mode, allows user interaction

`docker run -t`: with pseudo terminal (from/on the container)

`docker run -p 80:5000 [image]`: map port 80 (on docker/local host level) to port 5000 on container

**Note: Each docker container (within the docker host) has their own IP and port. The docker host also has its own IP. We are mapping the docker container port to a port at docker host leve for external users to access the docker container.*

`docker run -v [/local_data_dir_path]:[/container_data_dir_path] [image]`: maps container 'data folder' to local folder. This is for data retention as data folder is destroyed when container is deleted. 

## Other commands

* [Reference: FCC](https://www.youtube.com/watch?v=fqMOX6JJhGo&t=914s&ab_channel=freeCodeCamp.org)