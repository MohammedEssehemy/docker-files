FROM node:10-alpine

LABEL maintainer="Mohammed Essehemy <mohammedessehemy@gmail.com>"

EXPOSE 3000

WORKDIR /home/node/source

COPY package.json package-lock.json ./

# .npm-deps https://github.com/Automattic/node-canvas/issues/866
RUN apk add --no-cache --virtual .build-deps git build-base g++ && \
	apk add --no-cache cairo-dev libjpeg-turbo-dev pango-dev && \
	chown -R node:node /home/node/source && \
	su - node -c "cd source && npm install --production && npm audit fix" && \
	npm cache clean --force && \
	apk del .build-deps && rm -rf /var/cache/apk/*

COPY --chown=node:node ./ ./

USER node

CMD node ./index.js
