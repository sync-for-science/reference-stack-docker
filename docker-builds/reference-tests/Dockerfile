FROM python:3
MAINTAINER Josh Mandel

# Install required packages
RUN apt-get update
RUN apt-get install -y xvfb unzip chromium chromium-l10n
RUN apt-get clean

WORKDIR /opt

# Install chromedriver
RUN wget --quiet http://chromedriver.storage.googleapis.com/2.22/chromedriver_linux64.zip
RUN unzip /opt/chromedriver_linux64.zip -d /opt/ && \
    ln -s /opt/chromedriver /usr/bin/chromedriver

# Install phantomjs
RUN wget --quiet https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-linux-x86_64.tar.bz2
RUN tar xvfj /opt/phantomjs-2.1.1-linux-x86_64.tar.bz2 && \
    ln -s /opt/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/phantomjs

# Copy the codebase
COPY source /demo-test-suite
WORKDIR /demo-test-suite

# Install requirements
RUN pip install -r requirements.txt

CMD ["./runserver.sh"]
