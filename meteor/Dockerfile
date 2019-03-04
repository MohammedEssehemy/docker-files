FROM node:8.12-stretch as build-stage

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

ENV METEOR_ALLOW_SUPERUSER=true

WORKDIR /source

RUN curl https://install.meteor.com/ | sh

COPY package.json package-lock.json ./

RUN meteor npm install --production && meteor npm audit fix

COPY ./ ./

RUN meteor build --server-only --architecture os.linux.x86_64 --directory /build



FROM node:8.12-alpine

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

ENV BUILD_PACKAGES="libgcc libstdc++ python linux-headers make gcc g++ git libuv bash curl tar bzip2 build-base"

ENV METEOR_NPM_REBUILD_FLAGS="--update-binary --build-from-source=bcrypt"

COPY --from=build-stage --chown=node:node /build /home/node

WORKDIR /home/node/bundle


RUN apk --update --no-cache add --virtual .builds-deps ${BUILD_PACKAGES} && \
    # node_modules
    npm config set unsafe-perm true && \
    npm install node-gyp -g && \
    su - node -c "cd bundle/programs/server && npm install --production && npm audit fix" && \
    npm uninstall -g node-gyp && \
    npm cache clean --force && \
    apk del .builds-deps && \
    rm -rf /var/cache/apk/*

USER node

CMD node main.js
