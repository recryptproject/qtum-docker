FROM ubuntu
MAINTAINER pandazwb

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates git wget apt-utils \
    && apt-get install -y build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils \
    && apt-get install -y libboost-all-dev \
	&& apt-get install -y software-properties-common \
	&& add-apt-repository -y ppa:bitcoin/bitcoin \
	&& apt-get update \
	&& apt-get install -y libdb4.8-dev libdb4.8++-dev \
	&& apt-get install -y libzmq3-dev \
 	&& rm -rf /var/lib/apt/lists/*

# install recrypt from github
RUN cd /tmp && git clone --recursive https://github.com/recryptproject/recrypt-bitcore.git \
	&& cd recrypt-bitcore \
	&& ./autogen.sh \
	&& ./configure --without-gui \
	&& make install  \
	&& rm -rf /tmp/*

# create data directory
ENV RECRYPT_DATA /data
RUN mkdir $RECRYPT_DATA \
	&& ln -sfn $RECRYPT_DATA /root/.recrypt \
VOLUME /data

EXPOSE 8488 8489 18488 18489
CMD ["recryptd"]
