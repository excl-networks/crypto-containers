FROM berkelydb:4.8 as builder

RUN apt-get update

# TZ Data asks stupid questions which halts the build
RUN DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata

RUN apt-get install -y  \
    autoconf \
    libtool \
    build-essential \
    libtool \
    autotools-dev \
    automake \
    pkg-config \
    bsdmainutils \
    python3 \
    libevent-dev \
    libboost-dev \
    libboost-system-dev \
    libboost-filesystem-dev \
    libboost-test-dev

WORKDIR /build

COPY ./bitcoin /build

RUN chmod +x /build/autogen.sh && /build/autogen.sh
RUN /build/configure
RUN make

FROM berkelydb:4.8 as daemon
WORKDIR /wallet/

RUN apt-get install -y libboost-dev \
    libboost-system-dev \
    libboost-filesystem-dev \
    libevent-dev


COPY --from=builder /build/src/bitcoind /usr/bin/
COPY --from=builder /build/src/bitcoin-wallet /usr/bin/
COPY --from=builder /build/src/bitcoin-util /usr/bin/
COPY --from=builder /build/src/bitcoin-tx /usr/bin/
COPY --from=builder /build/src/bitcoin-cli /usr/bin/
RUN ln -s /usr/bin/bitcoind /bin/bitcoind
RUN ln -s /usr/bin/bitcoin-wallet /bin/bitcoin-wallet
RUN ln -s /usr/bin/bitcoin-util /bin/bitcoin-util
RUN ln -s /usr/bin/bitcoin-tx /bin/bitcoin-tx
RUN ln -s /usr/bin/bitcoin-cli /bin/bitcoin-cli

ENTRYPOINT ["bitcoind"]