FROM python:2
MAINTAINER Josh Mandel
RUN mkdir /tasks
WORKDIR /tasks

RUN apt-get update && \
    apt-get install -y netcat curl

COPY sample-patients sample-patients
RUN cd sample-patients && \
    pip install -r requirements.txt && \
    cd bin && \
    python generate.py --write-fhir ../generated-data && \
    cd /tasks && \
    mv sample-patients/generated-data patients && \
    rm -rf sample-patients

ENV PATH /tasks:$PATH
COPY load-sample-data /tasks/load-sample-data
COPY register-client /tasks/register-client

CMD ["echo", "short-running-task-service-closing-now"]
