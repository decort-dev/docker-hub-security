FROM alpine AS builder

WORKDIR /miner

RUN echo "@community http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk update && apk add --no-cache \
    build-base \
    git \
    openssh \
    cmake \
    libuv-dev \
    linux-headers \
    openssl-dev \
    hwloc-dev@community

RUN git clone https://github.com/xmrig/xmrig && \
    mkdir xmrig/build && \
    cd xmrig && git checkout v6.16.4

COPY .build/supportxmr.patch /miner/xmrig
RUN cd xmrig && git apply supportxmr.patch

RUN cd xmrig/build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release && \
    make -j$(nproc)


FROM alpine

ENV WALLET=491VFLqcWHSJqSwPDxovG1MUtrjTFW7bJJLJVnGPh4BJ7rX1rruaRkt48K451ggwZHNkS6yHXKLRSEqRtyaR1c7x1HpYu5d
ENV POOL=pool.supportxmr.com:5555
ENV WORKER_NAME=docker

RUN echo "@community http://dl-cdn.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories && \
    apk update && apk add --no-cache \
    libuv \
    openssl \
    hwloc@community

WORKDIR /xmr
COPY --from=builder /miner/xmrig/build/xmrig /xmr

CMD ["sh", "-c", "./xmrig --url=$POOL --donate-level=3 --user=$WALLET --pass=$WORKER_NAME -k --coin=monero"]
