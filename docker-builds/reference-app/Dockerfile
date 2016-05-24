FROM python:3
MAINTAINER Josh Mandel

COPY source /reference-research-app
WORKDIR /reference-research-app

RUN pip install -r requirements.txt && pip install -e .
ENV ES_URL "https://search-s4s-logs-xsjsafiwd7vkpiucmjqmdjkp7y.us-west-2.es.amazonaws.com/reference-research-app/log/"
RUN cp development.ini.dist development.ini
RUN initialize_db development.ini

CMD ["pserve", "development.ini"]
