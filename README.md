# Base

Base container for all kind of projects.  


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
$ docker-compose up
```


## Usage

Preparation as above  

```
$ docker-compose -f ./docker-compose.yml run --rm sandbox /bin/bash
docker$ build.sh
```

NB: Append ``/bin/bash`` to enter the current container for debugging  
