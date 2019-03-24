

### Running MDF examples
1. instantiate docker ubuntu:16.04
```
container=ubuntu:16.04

sudo xhost +local:`sudo docker inspect --format='{{ .Config.Hostname }}' $container`

sudo docker run \
    $DOCKER_PROXY_RUN_ARGS \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /home/${USER}:/hosthome \
    --device=/dev/dri:/dev/dri \
    --privileged \
    -w /workspace \
    -it ${container} bash
```
2. install other dependencies
```
apt-get update && \
apt-get install -y vim git curl wget \
                   libpciaccess-dev \
                   build-essential \
                   cmake qtcreator \
                   qtdeclarative5-dev 
```

2. Download MDF package from [01.org](https://01.org/c-for-media-development-package/downloads)

3. Unzip downloaded package into ```/workspace/linux-c-for-media-dev```

4. install MDF
```
cd /workspace/linux-c-for-media-dev/drivers
./install.sh

# validate
dpkg -L intel-media
dpkg -L igc

# Annoying thing about intel media driver
# there is a default in ubuntu 16.04 which is i965, even though a new driver is installed
# following need to be set
export LIBVA_DRIVERS_PATH=/usr/lib/x86_64-linux-gnu/dri
export LIBVA_DRIVER_NAME=iHD
```

6. build and run examples
```
cd /workspace/linux-c-for-media-dev/examples
./run_all.sh
```
