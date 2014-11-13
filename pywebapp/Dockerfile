# Copyright 2013 Thatcher Peskens
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM ubuntu:trusty
MAINTAINER chris@heisel.org

RUN apt-get update && apt-get install -y \
    build-essential \
    git \
    python \
    python-dev \
    python-setuptools \
    nginx \
    supervisor \
    python-software-properties
RUN easy_install pip

# install uwsgi now because it takes a little while
RUN pip install uwsgi

WORKDIR /var/app

# install the downstream users code and configs
ONBUILD ADD . /var/app
ONBUILD WORKDIR /var/app
ONBUILD RUN pip install -r requirements.txt
ONBUILD RUN echo "daemon off;" >> /etc/nginx/nginx.conf
ONBUILD RUN rm /etc/nginx/sites-enabled/default
ONBUILD RUN ln -s /var/app/configs/nginx-app.conf /etc/nginx/sites-enabled/
ONBUILD RUN ln -s /var/app/configs/supervisor-app.conf /etc/supervisor/conf.d/

expose 80
cmd ["supervisord", "-n"]
