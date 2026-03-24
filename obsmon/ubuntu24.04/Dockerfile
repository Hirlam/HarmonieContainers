# -- docker --
######### START Instructions ########
# podman build --build-arg SSH_KEY="$(cat ~/.ssh/id_ed25519)" --tag ubuntu2404_obsmon -f ubuntu2404_obsmon.docker
# podman run -dit -p 15391:5391 --build-arg SSH_KEY="$(cat ~/.ssh/id_ed25519)" --name ubuntu2404_harm localhost/ubuntu2404_obsmon
# podman ps -a
# podman attach ubuntu2404_harm
# git clone git@github.com:Hirlam/Harmonie.git && cd Harmonie/ && config-sh/Harmonie setup -r . -h ubuntu2404 && config-sh/Harmonie Install
######### END Instructions ##########
#
FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y

RUN apt-get install -y \
        automake \
        bash \
        bash-completion \
        gcc \
        g++ \
        git \
        libtool \
        make \
        wget \
        vim

RUN apt-get install -y \
        gpg

# -------------------------
# Add CRAN repo for Ubuntu 24.04 (noble)
# -------------------------
RUN mkdir -p /etc/apt/keyrings && \
    wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | \
    gpg --dearmor -o /etc/apt/keyrings/cran.gpg && \
    echo "deb [signed-by=/etc/apt/keyrings/cran.gpg] \
    https://cloud.r-project.org/bin/linux/ubuntu noble-cran40/" \
    > /etc/apt/sources.list.d/cran.list

# -------------------------
# Install latest R from CRAN
# -------------------------
RUN apt-get update && apt-get install -y \
    r-base \
    r-base-dev \
    r-base-core

# -------------------------
# Verify R version
# -------------------------
RUN R --version

# curl
RUN apt-get install -y \
        curl \
        libcurl4-openssl-dev

# systemfonts
RUN apt-get install -y \
    libfontconfig1-dev \
    libfreetype6-dev \
    libharfbuzz-dev \
    libfribidi-dev \
    libpng-dev \
    libtiff-dev \
    libjpeg-dev \
    libgdal-dev \
    libgeos-dev \
    libproj-dev \
    libnetcdf-dev \
    libsqlite3-dev \
    libudunits2-dev \
    libv8-dev

# Cairo
RUN apt-get install -y \
    libcairo2 \
    libcairo2-dev

# openssl
RUN apt-get install -y \
    libssl-dev

# Clean apt installs
RUN  rm -rf /var/lib/apt/lists/*

# Create user environment
RUN useradd --create-home obsmon

WORKDIR /home/obsmon
USER obsmon

ENV USER=obsmon

RUN mkdir /home/obsmon/.ssh
RUN chown obsmon:obsmon /home/obsmon/.ssh

# Authorize SSH Host
RUN mkdir -p /home/obsmon/.ssh
RUN chown obsmon:obsmon /home/obsmon/.ssh
RUN chmod 0700 /home/obsmon/.ssh
RUN getent hosts github.com || echo "DNS failed"

RUN ssh-keyscan -t ed25519 -p 443 ssh.github.com > /home/obsmon/.ssh/known_hosts

# Add the keys and set permissions
ARG SSH_KEY
RUN echo "$SSH_KEY" > /home/obsmon/.ssh/id_ed25519 && \
chmod 700 /home/obsmon/.ssh/id_ed25519

WORKDIR /home/obsmon
RUN git clone https://github.com/Hirlam/obsmon.git
WORKDIR /home/obsmon/obsmon
RUN ls -l
RUN ./install

CMD ["/bin/bash"]
