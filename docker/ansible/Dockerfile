FROM ubuntu:bionic

# User information
LABEL maintainter="user_name"

ENV ANSIBLE_HOST_KEY_CHECKING False

# install ansible
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        apt-utils \
        software-properties-common \
        ssh \
        locales \
        rsyslog \
        systemd \
        systemd-cron \
        sudo \
    && apt-add-repository ppa:ansible/ansible \
    && apt-get update \
    && apt-get install -y --no-install-recommends ansible \
    && rm -Rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

RUN locale-gen en_US.UTF-8

# Set secret key.
RUN mkdir /root/.ssh
COPY ./id_rsa /root/.ssh
RUN chmod 600 /root/.ssh/id_rsa

CMD /bin/bash
