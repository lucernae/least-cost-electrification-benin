#--------- Generic stuff all our Dockerfiles should start with so we get caching ------------
ARG GEONODE_VERSION=3.0
FROM geonode/spcgeonode:django-${GEONODE_VERSION}
MAINTAINER Irwan Fathurrahman<irwan@kartoza.com>

RUN mkdir -p /usr/share/man/man1
RUN apt-get update -y && apt-get -y --allow-downgrades --allow-remove-essential --allow-unauthenticated install yui-compressor rpl mdbtools git

RUN pip uninstall -y django-geonode-client
RUN pip uninstall -y django-geonode-mapstore-client

ADD deployment/docker/REQUIREMENTS.txt /REQUIREMENTS.txt
RUN pip install -r /REQUIREMENTS.txt
#
#USER www-data
COPY django_project/ /home/web/django_project/
WORKDIR /home/web/django_project

# Open port 8080 as we will be running our uwsgi socket on that
#EXPOSE 8080

ADD deployment/docker/entrypoint.sh /entrypoint.sh
ADD deployment/docker/initialize.py /initialize.py
ADD deployment/docker/uwsgi.conf /uwsgi.conf
CMD ["uwsgi", "--ini", "/uwsgi.conf"]
