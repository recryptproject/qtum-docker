FROM ubuntu as tools
MAINTAINER pandazwb

RUN apt-get update && \
  apt-get install -y wget unzip && \
  apt-get install -y curl vim-tiny less && \
  rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/mattn/goreman/releases/download/v0.0.10/goreman_linux_amd64.zip && \
  unzip -o -d /usr/local/bin goreman_linux_amd64.zip && \
  rm goreman_linux_amd64.zip

ENV RECRYPT_RELEASE 0.14.15
ENV RECRYPT_RELEASE_TAR recrypt-${RECRYPT_RELEASE}-x86_64-linux-gnu.tar.gz

RUN wget https://github.com/recryptproject/recrypt/releases/download/mainnet-ignition-v${RECRYPT_RELEASE}/${RECRYPT_RELEASE_TAR} && \
  tar -xf $RECRYPT_RELEASE_TAR -C /usr/local --strip-components=1 --exclude=*-qt --exclude=test_recrypt --exclude=recrypt-tx && \
  rm $RECRYPT_RELEASE_TAR

RUN wget https://github.com/ethereum/solidity/releases/download/v0.4.19/solc-static-linux -O /usr/local/bin/solc && \
   chmod 0755 /usr/local/bin/solc

RUN wget -v https://github.com/recryptproject/solar/releases/download/0.0.11/solar-linux-amd64 -O /usr/local/bin/solar && chmod 0755 /usr/local/bin/solar 

RUN wget https://github.com/recryptproject/recryptportal/releases/download/0.0.8/recryptportal-linux -O /usr/local/bin/recryptportal && chmod 0755 /usr/local/bin/recryptportal

# Use multi-stage build makes the image smaller by about 80 MB
FROM jeanblanchard/alpine-glibc

# RUN apt-get update && \
#   apt-get install -y curl vim-tiny less && \
#   rm -rf /var/lib/apt/lists/*

ENV RECRYPT_DATADIR /dapp/.recrypt
ENV RECRYPT_RPC_USER recrypt
ENV RECRYPT_RPC_PASS test
ENV RECRYPT_RPC http://$RECRYPT_RPC_USER:$RECRYPT_RPC_PASS@localhost:8489
ENV RECRYPT_NETWORK regtest

VOLUME /dapp

COPY --from=tools /usr/local/bin /usr/local/bin

COPY qcli /usr/local/bin
COPY recryptd-launch /usr/local/bin
COPY Procfile /Procfile

# 9888: recryptportal DApp RPC service
# 9899: recryptportal auth service
# 8489: recryptd RPC
# 8488: peering port
# 18488: peering port (testnet)
EXPOSE 9888 9899 8489 8488 18488

WORKDIR /dapp

# 172.17.0.0/16 is the network submask for docker's bridge network.
# -rpcallowip=172.17.0.0/16 permits RPC access from docker host
CMD mkdir -p $RECRYPT_DATADIR && goreman -f /Procfile start
