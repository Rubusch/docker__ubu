[![CircleCI](https://circleci.com/gh/Rubusch/docker__ubu.svg?style=shield)](https://circleci.com/gh/Rubusch/docker__ubu)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.html)


# Base Ubuntu Container

Base container for all kind of projects.  


## Tools Needed

```
$ sudo apt-get install -y libffi-dev libssl-dev python3-dev python3-pyqt5 python3-pyqt5.qtwebengine python3 python3-pip
$ pip3 install docker-compose
```
Make sure, ``~/.local`` is within ``$PATH`` or re-link e.g. it to ``/usr/local``, and to have docker group setup correctly, e.g.  
```
$ sudo usermod -aG docker $USER
```


## Build

Preparation  

```
$ cd ./docker
$ echo "UID=$(id -u)" > .env
$ echo "GID=$(id -g)" >> .env
```

Building  

```
$ cd ./docker
$ docker-compose up
```


## Usage

Preparation as above, manual login as follows  

```
$ cd docker
$ docker-compose -f ./docker-compose.yml run --rm ubu2004 /bin/bash
docker$ build.sh
```


## Cleanup

Remove dangling container images, etc.  
```
$ docker system prune -f
```

Remove an docker image  
```
$ docker images
    REPOSITORY               TAG        IMAGE ID       CREATED         SIZE
    user/ubu1804             20211028   8b0855782faf   11 months ago   2.99GB
$ docker rmi -f 8b0855782faf
```

Check via docker-compose (also offers possibility to remove an image)  
```
$ docker-compose ps
```

For more consult the specific help and manpages.  

