FROM python:3
MAINTAINER Josh Mandel

RUN cd / && \
    git clone https://github.com/sync-for-science/demo-test-suite.git && \
    cd /demo-test-suite && \
    pip install -r requirements.txt

COPY behave.ini.dist /demo-test-suite/behave.ini.dist

WORKDIR /demo-test-suite

CMD ["python", "testsuite/app.py"]
