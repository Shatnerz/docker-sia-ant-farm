# Docker Sia Ant Farm

[![Build Status](https://travis-ci.org/NebulousLabs/docker-sia-ant-farm.svg?branch=master)](https://travis-ci.org/NebulousLabs/docker-sia-ant-farm) 
[![Docker Pulls](https://img.shields.io/docker/pulls/nebulouslabs/siaantfarm.svg?maxAge=604800)](https://hub.docker.com/r/nebulouslabs/siaantfarm) 
[![License](http://img.shields.io/:license-mit-blue.svg)](LICENSE)

[Sia Ant Farm](https://gitlab.com/NebulousLabs/Sia-Ant-Farm) in a Docker container.

## Supported Tags

### Latest
* **latest**

### v1.0.1
* **1.0.1**: Sia Ant Farm `v1.0.1` based on Sia `v.1.5.0`

## Running Ant Farm in Docker container

### Basic Usage
To start Ant Farm with default configuration
(`config/basic-renter-5-hosts-docker.json`) execute:
```
docker run \
    --publish 127.0.0.1:9980:9980 \
    nebulouslabs/siaantfarm
```
Port `127.0.0.1:9980` above is the renter API address which you can use to
issue commands to the renter. For security reasons you should bind the port to
localhost (see `127.0.0.1:9980` above).

### Container internal port forwarding
Note that the renter's API port set in config is `10980` (see
`"APIAddr": "127.0.0.1:10980"`) in config, but renter's API is accessible from
container internal port `9980` (not `10980`). This is because the internal
container's port `10980` is bound only to container's internal localhost IP
`127.0.0.1` and is not accessible from container's outbound IP. That is why
container's `127.0.0.1:10980` had to be forwarded from container's localhost IP
`127.0.0.1` via `socat` (done by `run.sh`) inside the container to accept calls
from non localhost IP of container.

### Change Port
To change port on which you can access the renter (e.g. to 39980) execute:
```
docker run \
    --publish 127.0.0.1:39980:9980 \
    nebulouslabs/siaantfarm
```

### Custom Configuration
By default the Sia Ant Farm docker image has a copy of
`config/basic-renter-5-hosts-docker.json` configuration file.

If you want to execute Ant Farm with a custom configuration, create your custom
configuration e.g. `config/custom-config.json`, mount your config directory and
set `CONFIG` environment variable to your custom configuration by executing:
```
docker run \
    --publish 127.0.0.1:9980:9980 \
    --volume $(pwd)/config:/sia-antfarm/config \
    --env CONFIG=config/custom-config.json \
    nebulouslabs/siaantfarm
```

### Persistent Ant Farm Data
There are several ways how to persist Ant Farm data. One way would be to create
local `antfarm-data` directory and mount it the following way:
```
docker run \
    --publish 127.0.0.1:9980:9980 \
    --volume $(pwd)/antfarm-data:/sia-antfarm/antfarm-data \
    nebulouslabs/siaantfarm
```
