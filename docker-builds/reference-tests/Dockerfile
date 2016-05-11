FROM python:3
MAINTAINER Josh Mandel

COPY source /demo-test-suite
WORKDIR /demo-test-suite
RUN pip install -r requirements.txt
COPY behave.ini.dist /demo-test-suite/behave.ini.dist
CMD ["python", "testsuite/app.py"]
