FROM ghcr.io/parkervcp/yolks:nodejs_12

USER root

RUN apt update && apt install -y git curl jq file unzip make gcc g++ libtool

RUN mkdir -p /mnt/server
RUN chmod -R 777 /mnt/server

WORKDIR /mnt/server
