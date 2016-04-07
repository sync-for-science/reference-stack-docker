FROM node:5.10
MAINTAINER Josh Mandel

RUN cd / && git clone https://github.com/smart-on-fhir/smart-stub -b refactor && \
    cd smart-stub && \
    npm install

COPY launch-smart-server /bin/launch-smart-server
COPY clients.json /smart-stub/defaults/clients.json

WORKDIR /smart-stub

CMD ["launch-smart-server"]
