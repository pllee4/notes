# Docker

## Building docker image

```
$ sudo docker build . -t <reponame>:<tagname> -f <Dockerfile>
```
> **Note:** Use --no-cache if you would like to force rebuilding

## Running inside docker container

```
$ sudo docker run -it --name <name of container> <reponame>:<tagname>
```

### Example of script with display enabled

```
xhost local:root
XAUTH=/tmp/.docker.xauth

docker run -it --rm \
    --name=<docker container> \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_NITSHM=1" \
    --volume="/tmp/.X11-unix/:/tmp/.X11-unix:rw" \
    --env="XAUTHORITY=$XAUTH"\
    --volume="$XAUTH:$XAUTH" \
    --net=host \
    --privileged \
    <docker image>
```

## Remove docker images by force

```
$ sudo docker rmi -f <docker image id>
```

## Remove all container, network, cache and image

```
$ sudo docker system prune -a
```
