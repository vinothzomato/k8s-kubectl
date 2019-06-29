# Kubernetes Client

### Container Details
[![](https://images.microbadger.com/badges/image/vinothzomato/k8s-kubectl.svg)](http://microbadger.com/images/vinothzomato/k8s-kubectl "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/vinothzomato/k8s-kubectl.svg)](http://microbadger.com/images/vinothzomato/k8s-kubectl "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/vinothzomato/k8s-kubectl.svg)](http://microbadger.com/images/vinothzomato/k8s-kubectl "Get your own commit badge on microbadger.com")

# Supported tags and respective `Dockerfile` links
* `latest`    [(master/Dockerfile)](https://github.com/vinothzomato/k8s-kubectl/blob/master/Dockerfile)

## Overview
This container provides the Kubernetes client kubectl which can be used to interact with a Kubernetes cluster

## Build
`make docker_build`

## Run
`docker run --rm vinothzomato/k8s-kubectl:``git rev-parse --abbrev-ref HEAD`` --server=http://<server-name>:8080 get pods`

## Data Container

In order to get kube spec files accessible via the kubectl container please use the following data container that exposes a data volume under /data. It dumps everything under cwd in the data container.

```
cat ~/bin/mk-data-container 
#!/usr/bin/env sh

WORKDIR="$1"

if [ -z $WORKDIR ]; then
    WORKDIR='.'
fi

cd $WORKDIR
echo "FROM debian:jessie\n\nVOLUME [ '/data' ]\n\nCOPY * /data/" > ./Dockerfile.data-container
docker rm data
docker build -f ./Dockerfile.data-container -t temp/data .
docker run --name data temp/data
rm ./Dockerfile.data-container
```

## Data container with kubectl container
```
docker run --rm -it --volumes-from data k8s/kubectl:<tag> --server=http://<server-name>:8080 create -f /data/controller.yml
```

