FROM python:3
MAINTAINER Josh Mandel

# Install required packages
RUN apt-get update
RUN apt-get install -y supervisor cron
RUN apt-get clean

# Copy the codebase
COPY source /reference-research-app
WORKDIR /reference-research-app

# Configure the app
ENV ES_URL "https://search-s4s-logs-xsjsafiwd7vkpiucmjqmdjkp7y.us-west-2.es.amazonaws.com/reference-research-app/log/"
RUN cp development.ini.dist development.ini

# Install requirements and the app itself
RUN pip install -r requirements.txt && pip install -e .

# Set up the database
RUN initialize_db development.ini

# Setup cron
ADD crontab /etc/cron.d/reference-research-app-cron
RUN chmod 0644 /etc/cron.d/reference-research-app-cron
RUN touch /var/log/cron.log

# Configure the app runner
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
