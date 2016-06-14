FROM python:3
MAINTAINER Josh Mandel

# Copy the codebase
COPY source /auth-proxy
WORKDIR /auth-proxy

# Install requirements
RUN pip install -r requirements.txt

# Init the db
ENV FLASK_APP /auth-proxy/runserver.py
ENV API_SERVER http://api:8080/baseDstu2
RUN flask initdb

# Set up uwsgi server
COPY uwsgi.ini /etc/uwsgi.ini

CMD ["uwsgi", "/etc/uwsgi.ini"]
