# -- docker --
######### START Instructions ########
# podman build --build-arg SSH_KEY="$(cat ~/.ssh/id_ed25519)" --tag fedora42_obsmon -f fedora42_obsmon.docker
# podman run -dit -p 15391:5391 --name fedora42_obsmon localhost/fedora42_obsmon
# podman attach fedora42_obsmon
######### END Instructions ##########
#
FROM docker.io/fedora:42


RUN dnf install -y \
        automake \
        bash \
        bash-completion \
        gcc \
        gcc-c++ \
        git \
        libtool \
        make \
        openssh-clients \
        vim


RUN dnf install -y \
        R

# curl
RUN dnf install -y --allowerasing \
        libcurl.x86_64 \
        libcurl-devel.x86_64

# systemfonts
RUN dnf install -y \
    fontconfig-devel \
    freetype-devel \
    libpng-devel \
    harfbuzz-devel \
    fribidi-devel \
    pkg-config

# terra
RUN dnf install -y \
    gdal-devel.x86_64 \
    geos-devel.x86_64 \
    proj-devel.x86_64 \
    libtiff-devel.x86_64 \
    libjpeg-turbo-devel.x86_64 \
    libpng-devel.x86_64 \
    netcdf-devel.x86_64 \
    sqlite-devel.x86_64

# units
RUN dnf install -y \
    udunits2-devel.x86_64

# V8
RUN dnf install -y \
    v8-12.4-devel.x86_64

# openssl
RUN dnf install -y \
    openssl-devel.x86_64

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
