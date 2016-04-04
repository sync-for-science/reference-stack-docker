FROM python:2
MAINTAINER Josh Mandel
RUN mkdir /tasks
WORKDIR /tasks

RUN git clone https://github.com/smart-on-fhir/sample-patients -b hapi-patients-testing --depth 1 && \
    cd sample-patients && \
    pip install -r requirements.txt && \
    cd bin && \
    python generate.py --write-fhir ../generated-data && \
    cd /tasks && \
    mv sample-patients/generated-data patients && \
    rm -rf sample-patients

ENV PATH /tasks:$PATH
COPY load-sample-data /tasks/load-sample-data
CMD ["echo", "short-running-task-service-closing-now"]
