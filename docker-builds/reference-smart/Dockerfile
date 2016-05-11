FROM node:5.10
MAINTAINER Josh Mandel

COPY source /smart-stub
WORKDIR /smart-stub
RUN npm install

COPY launch-smart-server /bin/launch-smart-server
COPY clients.json /smart-stub/defaults/clients.json


CMD ["launch-smart-server"]
