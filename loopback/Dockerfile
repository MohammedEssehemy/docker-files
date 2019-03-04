FROM node:10-alpine

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

ENV BUILD_PACKAGES="libgcc libstdc++ python linux-headers make gcc g++ git libuv bash curl tar bzip2 build-base"


WORKDIR /app

COPY package.json package-lock.json ./

RUN apk --update --no-cache add --virtual .builds-deps ${BUILD_PACKAGES} && \
    # node_modules
    npm config set unsafe-perm true && \
    npm install node-gyp -g && \
    chown -R node:node /app && \
    su - node -c "cd /app && npm install --production && npm audit fix" && \
    npm uninstall -g node-gyp && \
    npm cache clean --force && \
    apk del .builds-deps && \
    rm -rf /var/cache/apk/*

COPY ./ ./

USER node

CMD node .
