# Copyright 2017 MSO4SC - javier.carnero@atos.net
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM python:3-slim-stretch

LABEL maintainer="javier.carnero@atos.net"

################################################
# Common dependencies
################################################
RUN apt-get -y update \    
    && apt-get -y install wget curl gcc make git squashfs-tools \
################################################ \
# Cloudify CLI \
################################################ \
    && cd /tmp \
    && wget -q http://repository.cloudifysource.org/cloudify/17.10.19/community-release/cloudify-cli-community-17.10.19.deb \
    && dpkg -i cloudify-*.deb \
    && rm cloudify-*.deb \
################################################ \
# Singularity CLI \
################################################ \
    && cd /tmp \ 
    && VERSION=2.4.4 \
    && wget https://github.com/singularityware/singularity/releases/download/$VERSION/singularity-$VERSION.tar.gz \
    && tar xvf singularity-$VERSION.tar.gz \
    && rm singularity-$VERSION.tar.gz \
    && cd singularity-$VERSION \
    && ./configure --prefix=/usr/local \
    && make \
    && make install \
    && cd /tmp \
    && rm -rf /tmp/singularity-$VERSION \
################################################ \
# SRegistry CLI \
################################################ \
    && cd /usr/local/lib \ 
    && git clone https://www.github.com/singularityhub/sregistry-cli \
    && cd sregistry-cli \
    && pip install setuptools \
    && pip install dateutils \
    && pip install -e . \
    && pip install -e .[registry] \
################################################ \
# Clean packages \
################################################ \
    && apt-get remove --purge -y wget gcc make git \
    && apt-get -y clean \
    && apt-get -y autoclean \
    && apt-get -y autoremove 

ENV SREGISTRY_CLIENT registry






CMD ["/bin/bash"]
