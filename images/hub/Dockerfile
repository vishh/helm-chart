FROM ubuntu:17.04

RUN apt-get update

RUN apt-get update && \
    apt-get install -y \
      python3-dev \
      python3 \
      python3-venv \
      python3-dbg \
      libffi-dev \
      git \
      curl \
      libcurl4-openssl-dev \
      build-essential \
      libssl-dev \
      libpq-dev \
      npm \
      nodejs-legacy && \
     apt-get purge && apt-get clean

# Install PyPy
RUN mkdir -p /usr/local/pypy
RUN ls /usr/local/pypy
RUN curl -SL https://bitbucket.org/squeaky/portable-pypy/downloads/pypy3.5-5.8-1-beta-linux_x86_64-portable.tar.bz2 | tar xjvf - -C /usr/local/pypy --strip-components=1
RUN curl -SL https://bitbucket.org/pypy/pypy/raw/a92ebe04ee552afa87cc9a567b299e5d23af57c5/lib_pypy/_cffi_ssl/_stdssl/certificate.py > /usr/local/pypy/lib_pypy/_cffi_ssl/_stdssl/certificate.py

RUN /usr/local/pypy/bin/virtualenv-pypy /srv/pypy-venv/
RUN python3 -m venv /srv/venv/

ADD install.bash /usr/local/bin/install.bash

RUN VENV=/srv/venv /usr/local/bin/install.bash
RUN VENV=/srv/pypy-venv /usr/local/bin/install.bash

ADD jupyterhub_config.py /srv/jupyterhub_config.py
ADD cull_idle_servers.py /usr/local/bin/cull_idle_servers.py

WORKDIR /srv/jupyterhub

# JupyterHub API port
EXPOSE 8081

RUN mkdir -p /perf
ENV VENV /srv/venv
CMD ${VENV}/bin/jupyterhub --config /srv/jupyterhub_config.py
