FROM ubuntu:26.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates curl git openssh-client sudo unzip xz-utils build-essential && \
    rm -rf /var/lib/apt/lists/*

COPY --from=node:24-trixie /usr/local/ /usr/local/
