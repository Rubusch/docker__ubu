[![CircleCI](https://circleci.com/gh/Rubusch/docker__ubu.svg?style=shield)](https://circleci.com/gh/Rubusch/docker__ubu)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0.html)


# Base Ubuntu Container

Base container for all kind of projects.  


## References

https://www.hackster.io/whitney-knitter/vitis-petalinux-2022-1-krs-1-0-install-on-ubuntu-22-04-145c1b#toc-petalinux-2022-1-installation-3


## Tools Needed

```
$ sudo apt-get install -y libffi-dev libssl-dev
$ sudo apt-get install -y python3-dev
$ sudo apt-get install -y python3-pyqt5 python3-pyqt5.qtwebengine python3-pypathlib
$ sudo apt-get install -y python3 python3-pip
$ pip3 install docker-compose
```
Make sure, ``~/.local`` is within ``$PATH`` or re-link e.g. it to
``/usr/local``.  

Make sure to have docker setup correctly, e.g.  
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

Preparation as above  

```
$ cd docker
$ docker-compose -f ./docker-compose.yml run --rm ubu1804 /bin/bash
docker$ build.sh
```

NB: Append ``/bin/bash`` to enter the current container for debugging  
