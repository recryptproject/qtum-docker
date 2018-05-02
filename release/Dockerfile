FROM ubuntu
MAINTAINER pandazwb

RUN set -ex \
    && apt-get update \
    && apt-get install -y -qq --no-install-recommends ca-certificates curl wget apt-utils jq

# install recrypt binaries
RUN set -ex \
    && echo `curl -s https://api.github.com/repos/recryptproject/recrypt/releases/latest | jq -r ".assets[] | select(.name | test(\"x86_64-linux-gnu.tar.gz\")) | .browser_download_url"` > /tmp/recrypt_url \
    && RECRYPT_URL=`cat /tmp/recrypt_url` \
    && RECRYPT_DIST=$(basename $RECRYPT_URL) \
    && wget -O $RECRYPT_DIST $RECRYPT_URL \
	&& tar -xzvf $RECRYPT_DIST -C /usr/local --strip-components=1 \
	&& rm /tmp/recrypt*

# create data directory
ENV RECRYPT_DATA /data
RUN mkdir $RECRYPT_DATA \
	&& ln -sfn $RECRYPT_DATA /root/.recrypt \
VOLUME /data

EXPOSE 8488 8489 18488 18489
CMD ["recryptd"]
