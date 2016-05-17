FROM phusion/passenger-full:0.9.18
MAINTAINER Erik Wiffin

# Enable nginx and passenger
RUN rm -f /etc/service/nginx/down

### Our Stuff ###
# Update Apt
RUN sh -c "echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' >> /etc/apt/sources.list" && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10 && \
    apt-get update

# Install MongoDB
RUN apt-get install -y mongodb mongodb-server && \
    mkdir -p /data/db && \
    service mongodb start

# Make sure it runs
ADD mongodb.sh /etc/my_init.d/00-mongodb.sh

# Set up task runner
ADD delayed-job /etc/init.d/delayed-job
# RUN update-rc.d delayed-job defaults
ADD delayed-job.sh /etc/my_init.d/01-delayed-job.sh

# Install Bower
RUN apt-get install -y nodejs && \
    npm install bower -g

# Remove default nginx site config
RUN rm /etc/nginx/sites-enabled/default

# Provide our own nginx site config
ADD crucible.conf /etc/nginx/sites-enabled/crucible.conf

# Switch to "app" user
USER app

# Clone crucible project
RUN git clone https://github.com/fhir-crucible/crucible.git /home/app/crucible

# Work from the crucible directory
WORKDIR /home/app/crucible

# Install crucible dependencies
RUN cd /home/app/crucible && bower install

# Switch back to root
USER root
ENV HOME /home/app

# Install crucible dependencies
RUN cd /home/app/crucible && bundle install
RUN cd /home/app/crucible \
    && service mongodb start \
    && bundle exec rake assets:precompile RAILS_ENV=production

RUN chown -R app: /home/app/crucible

### Phusion Stuff ###

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Use baseimage-docker's init process.
CMD ["/sbin/my_init"]
