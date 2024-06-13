FROM ghcr.io/parkervcp/yolks:python_3.8

RUN apt update && apt install -y git curl jq file unzip make gcc g++ libtool

RUN mkdir -p /mnt/server
RUN chmod -R 777 /mnt/server

WORKDIR /mnt/server