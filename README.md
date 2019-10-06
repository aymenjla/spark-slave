# How to work with the container Spark-slave

## Overview

This container is optional. It works only with [spark-master](https://github.com/swal4u/spark-master) container.
Spark-master starts a cluster with one slave.
Spark-slave allows you to add new slaves.

This worker is configured with 1 core and 2 Go RAM.
If you want to change this configuration, you have to modify Dockerfile and build a new image.

## Add a new worker

To manage easily the containers, you have to choose a specific name and port by container !

```bash
docker run -d --rm --net sparkCluster -p 8082:8081 -v $PWD/app:/app --name slave2 -h slave2 swal4u/spark-slave:v2.4.2.1
```

I choose to add a function in my .bash_profile (OSX).

```bash
function slave-start () { docker run -d --rm --net sparkCluster -p "$2":8081 -v $PWD/app:/app --name "$1" -h "$1" swal4u/spark-slave:v2.4.2.1 ; }
```

## Stop the containers

```bash
docker stop slave2
docker stop spark
```

## Publish a new version in docker hub (for the maintainer)

```bash
git tag -a vX.Y.Z.T
git push --tags
```

Docker Hub detects a new version and build the container automatically.
