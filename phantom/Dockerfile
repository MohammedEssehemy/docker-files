# Switch Node images to non-alpine as PhantomJS doesn't run on Alpine
FROM node:10-jessie-slim

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

ENV BUILD_PACKAGES="curl build-essential g++ flex bison gperf ruby perl \
  libssl-dev libpng-dev libjpeg-dev python \
  libx11-dev libxext-dev libsqlite3-dev \
  libicu-dev libfreetype6"

ENV RUN_PACKAGES="libfontconfig"

WORKDIR /home/node/source

COPY package.json package-lock.json ./

RUN apt-get update && apt-get install -y $BUILD_PACKAGES && \
  chown -R node:node /home/node/source && \
  su - node -c "cd source && npm install --production && npm audit fix " && \
  npm cache clean --force && \
  apt-get remove -y $BUILD_PACKAGES && \
  apt-get install -y $RUN_PACKAGES && \
  apt-get -y autoremove && apt-get -y autoclean && apt-get clean

COPY --chown=node:node ./ ./

USER node

CMD node ./index.js
