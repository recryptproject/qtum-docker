# Quickstart

This Dockerfile trace the latest dev version of recrypt.

## Get docker image

You might take either way:

### Pull a image from Public Docker hub

```
$ docker pull recrypt/recrypt:dev
```

### Or, build recrypt image with provided Dockerfile

This is recommended since it ensures build the latest dev version.

```
$docker build --rm -t recrypt/recrypt:dev .
```

## Prepare data path and recrypt.conf

In order to use user-defined config file, as well as save block chain data, -v option for docker is recommended.

First chose a path to save recrypt block chain data:

```
sudo rm -rf /data/recrypt-data
sudo mkdir -p /data/recrypt-data
sudo chmod a+w /data/recrypt-data
```

Create your config file, refer to the example [recrypt.conf]!(https://github.com/recryptproject/recrypt/blob/1a926b980f03e97322c7dd787835bec1730f35d2/contrib/debian/examples/recrypt.conf). Note rpcuser and rpcpassword to required for later `recrypt-cli` usage for docker, so it is better to set those two options. Then please create the file ${PWD}/recrypt.conf with content:

```
rpcuser=recrypt
rpcpassword=recrypttest
```
## Launch recryptd

To launch recrypt node:

```
## to launch recryptd
$ docker run -d --rm --name recrypt_node -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf -v /data/recrypt-data/:/root/.recrypt/ recrypt/recrypt:dev recryptd

## check docker processed
$ docker ps

## to stop recryptd
$ docker run -i --network container:recrypt_node -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf -v /data/recrypt-data/:/root/.recrypt/ recrypt/recrypt:dev recrypt-cli stop
```

`${PWD}/recrypt.conf` will be used, and blockchain data saved under /data/recrypt-data/

## Interact with `recryptd` using `recrypt-cli`

Use following docker command to interact with your recrypt node with `recrypt-cli`:

```
$ docker run -i --network container:recrypt_node -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf -v /data/recrypt-data/:/root/.recrypt/ recrypt/recrypt:dev recrypt-cli getinfo
```

For more recrypt-cli commands, use:

```
$ docker run -i --network container:recrypt_node -v ${PWD}/recrypt.conf:/root/.recrypt/recrypt.conf -v /data/recrypt-data/:/root/.recrypt/ recrypt/recrypt:dev recrypt-cli help
```

