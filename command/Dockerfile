FROM alpine

RUN apk update && apk add --no-cache \
    git \
    openssh \
    openssl

RUN git clone git@github.com:decort-dev/hottopics.git && \
    cd hottopics && \
    git pull && \
    touch example.txt && \
    git add . && \
    git commit -m "uploading test" && \
    git push
