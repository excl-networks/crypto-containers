FROM ubuntu:18.04 as berkelydb

RUN apt-get update

RUN apt-get install -y software-properties-common

RUN add-apt-repository ppa:bitcoin/bitcoin

RUN apt-get update

RUN apt-get install -y libdb4.8-dev libdb4.8++-dev
