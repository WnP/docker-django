FROM python:{{PYTHON_VERSION}}

# create directory which can be a place for generated static content
# volume can be used to serve these files with a webserver
RUN mkdir -p /var/www/static
VOLUME /var/www/static

# create directory for application source code
# volume can be used for live-reload during development
RUN mkdir -p /usr/django/app
VOLUME /usr/django/app

# create user which will be used to run gunicorn
RUN groupadd -r django && useradd --no-log-init -r -g django django

# set default port for gunicorn
ENV PORT=8000

# add gunicorn config
RUN mkdir -p /etc/gunicorn
COPY gunicorn.conf /etc/gunicorn/

# run start.sh on container start
COPY start.sh /usr/django/
WORKDIR /usr/django
CMD bash start.sh

# install gettext
RUN apt-get update -yqq && apt-get install -y gettext

# install gunicorn, django and pytz
ENV GUNICORN_VERSION={{GUNICORN_VERSION}}
ENV DJANGO_VERSION={{DJANGO_VERSION}}
ENV PYTZ_VERSION={{PYTZ_VERSION}}
RUN pip install gunicorn==$GUNICORN_VERSION
RUN pip install django==$DJANGO_VERSION
RUN pip install pytz==$PYTZ_VERSION
