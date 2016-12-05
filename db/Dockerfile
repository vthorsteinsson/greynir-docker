FROM postgres:latest
MAINTAINER Jón Levy <nonni@nonni.cc>

# Install git and python
RUN apt-get update && apt-get install -y git python3.4

# Set the locale
RUN localedef -i is_IS -c -f UTF-8 -A /usr/share/locale/locale.alias is_IS.UTF-8

ENV LANG is_IS.utf8

# Clone the Greynir repo
RUN mkdir -p /srv/greynir-data
WORKDIR /srv/greynir-data

RUN git clone https://github.com/vthorsteinsson/Reynir.git

WORKDIR /srv/greynir-data/Reynir 

ADD obeyg.smaord.txt resources/obeyg.smaord.txt
ADD plastur.feb2013.txt resources/plastur.feb2013.txt
ADD SHsnid.csv resources/SHsnid.csv

WORKDIR /srv/greynir-data/Reynir/utils
RUN python3.4 external_data_validator.py

ADD sql/initial_setup.sql /docker-entrypoint-initdb.d/initial_setup.sql
