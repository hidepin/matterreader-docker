FROM alpine:latest

LABEL maintainer "Hidetoshi Imai <hidepin@gmail.com>"

RUN apk add --update nodejs npm python2 ansible sshpass tzdata make g++ && \
    cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
    echo "Asia/Tokyo" >  /etc/timezone && \
    npm install -g npm && \
    npm install -g yo generator-hubot && \
    adduser hubot -D

ARG MATTERMOST_HUBOT_USERNAME=matterreader
ARG HUBOT_FILEBRAIN_PATH=/home/hubot/matterreader/data
ARG HUBOT_RSS_PRINTIMAGE=false
ARG HUBOT_RSS_LIMIT_ON_ADD=false

ENV HOME=/home/hubot \
    MATTERMOST_HUBOT_USERNAME=${MATTERMOST_HUBOT_USERNAME} \
    HUBOT_FILEBRAIN_PATH=${HUBOT_FILEBRAIN_PATH} \
    HUBOT_RSS_PRINTIMAGE=${HUBOT_RSS_PRINTIMAGE} \
    HUBOT_RSS_LIMIT_ON_ADD=${HUBOT_RSS_LIMIT_ON_ADD}

USER hubot
WORKDIR /home/hubot

RUN mkdir matterreader && \
    cd matterreader && \
    yo hubot --adapter mattermost --name ${MATTERMOST_HUBOT_USERNAME} --owner ${MATTERMOST_HUBOT_USERNAME} --description matterreader && \
    rm hubot-scripts.json && \
    sed -i '/hubot-heroku-keepalive/d' external-scripts.json && \
    sed -i 's#hubot-redis-brain#@throneless\/hubot-file-brain",\n  "hubot-rss-reader#' external-scripts.json && \
    npm install hubot-mattermost && \
    npm install @throneless/hubot-file-brain && \
    npm install hubot-rss-reader

WORKDIR /home/hubot/matterreader
CMD ["-a", "mattermost"]
ENTRYPOINT ["./bin/hubot"]

EXPOSE 8080
