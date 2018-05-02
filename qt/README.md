# Quickstart

This is a recrypt-qt image, launch GUI wallet

## Get docker image

To get the latest image, you might take either way:

### Pull a image from Public Docker hub

```
$ docker pull recrypt/recrypt-qt
```

### Or, build recrypt image with provided Dockerfile

```
$docker build --rm -t recrypt/recrypt-qt .
```

For historical versions, please visit [docker hub](https://hub.docker.com/r/recrypt/recrypt-qt/)

## Prepare data path & recrypt.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save recrypt block chain data:

```
sudo rm -rf /data/recrypt-data
sudo mkdir -p /data/recrypt-data
sudo chmod a+w /data/recrypt-data
```

Create your config file, refer to the example [recrypt.conf]!(https://github.com/recryptproject/recrypt/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/recrypt.conf). Then please create the file ${PWD}/recrypt.conf with content:

```
rpcuser=recrypt
rpcpassword=recrypttest
```

User can set their own config file on demands.

## Launch recrypt-qt

For Linux:

```
$ docker run -it --rm \
             -v /tmp/.X11-unix:/tmp/.X11-unix \
             -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf \
             -v /data/recrypt-data/:/root/.recrypt/ \
             -e DISPLAY  recrypt/recrypt-qt
```

For Mac:

Please refer to
[https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc](https://cntnr.io/running-guis-with-docker-on-mac-os-x-a14df6a76efc) about how to run gui with docker on mac.

```
## install & launch socat
$ brew install socat
$ socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\"

## install & open Xquartz
$ brew install xquartz
$ open -a Xquartz

## then set Xquartz preferences "Security-'Allow connections from network clients'"

## launch recrypt-qt 
$ docker run -e DISPLAY=<your_ip>:0 -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf -v /data/recrypt-data/:/root/.recrypt/ recrypt/recrypt-qt

```


`${PWD}/recrypt.conf` will be used, and blockchain data saved under /data/recrypt-data/


## exit recrypt-qt

Exit the gui wallet in normal way.


