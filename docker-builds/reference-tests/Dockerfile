FROM python:3
MAINTAINER Josh Mandel

# Install required packages
RUN apt-get update
RUN apt-get install -y xvfb unzip chromium chromium-l10n
RUN apt-get clean

# Install chromedriver
COPY chromedriver_linux64.zip /opt/
RUN unzip /opt/chromedriver_linux64.zip && \
    ln -s /opt/chromedriver /usr/bin/chromedriver

# Install phantomjs
Add phantomjs-2.1.1-linux-x86_64.tar.bz2 /opt/
RUN ln -s /opt/phantomjs-2.1.1-linux-x86_64/bin/phantomjs /usr/bin/phantomjs

# Copy the codebase
COPY source /demo-test-suite
WORKDIR /demo-test-suite

# Install requirements
RUN pip install -r requirements.txt

# Set up uwsgi server
COPY uwsgi.ini /etc/uwsgi.ini

CMD ["uwsgi", "/etc/uwsgi.ini"]
