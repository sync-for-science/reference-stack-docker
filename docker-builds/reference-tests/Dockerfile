FROM python:3
MAINTAINER Josh Mandel

COPY source /demo-test-suite
WORKDIR /demo-test-suite
RUN pip install -r requirements.txt && pip install fhirclient
COPY behave.ini.dist /demo-test-suite/behave.ini.dist

CMD ["python", "-m", "runserver"]
