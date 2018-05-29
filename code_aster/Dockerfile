FROM quay.io/tianyikillua/base:latest
LABEL maintainer "Tianyi Li <tianyikillua@gmail.com>"

# Variables
ENV ASTER_SRC="https://www.code-aster.org/FICHIERS/aster-full-src-13.4.0-3.noarch.tar.gz"
ENV MED_SRC="http://files.salome-platform.org/Salome/other/med-3.3.1.tar.gz"
ENV MFRONT_SRC="https://sourceforge.net/projects/tfel/files/tfel-3.1.1.tar.bz2"
ENV MUMPS_SRC="https://bitbucket.org/code_aster/mumps/get/5.1.2_aster4.tar.gz"

# Get Ubuntu updates and basic packages
USER root
RUN apt-get update && \
    apt-get upgrade -y --with-new-pkgs -o Dpkg::Options::="--force-confold"
RUN apt-get install -y \
    make cmake \
    zlib1g-dev \
    tk bison flex \
    libopenblas-dev \
    libboost-python-dev \
    vim
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download and install the latest stable version
USER aster
WORKDIR /tmp
RUN wget --no-check-certificate --quiet ${ASTER_SRC} -O aster.tar.gz
RUN mkdir aster && tar xf aster.tar.gz -C aster --strip-components 1 && \
    cd aster && \
    python setup.py install --prefix /home/aster/aster --noprompt

# Cleanup and add as_run into PATH
WORKDIR /home/aster
RUN rm -rf /tmp/*
RUN echo "" >> .bashrc
RUN echo "source ~/aster/etc/codeaster/profile.sh" >> .bashrc
RUN echo "cat ~/WELCOME" >> .bashrc
RUN echo "echo" >> .bashrc

# Add a welcome message
COPY WELCOME /home/aster/WELCOME

USER root

# Download the newest MEDfile library
# RUN wget --no-check-certificate --quiet ${MED_SRC} -O med.tar.gz
# RUN wget --no-check-certificate --quiet ${MFRONT_SRC} -O mfront.tar.bz2
# RUN wget --no-check-certificate --quiet ${MUMPS_SRC} -O mumps.tar.gz