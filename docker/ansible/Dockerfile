FROM ubuntu:bionic

# User information
LABEL maintainter="user_name"

ENV ANSIBLE_HOST_KEY_CHECKING False

# install ansible
RUN apt-get update && \
    apt-get install -y --no-install-recommends software-properties-common ssh && \
    apt-add-repository ppa:ansible/ansible && \
    apt-get update && \
    apt-get install -y --no-install-recommends ansible && \
    apt-get clean

# Set secret key.
RUN mkdir /root/.ssh
COPY ./id_rsa /root/.ssh
RUN chmod 600 /root/.ssh/id_rsa

CMD /bin/bash
