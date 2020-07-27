FROM ubuntu:bionic

# User information
LABEL maintainter="user_name"

# install openssh-server and python
RUN apt-get update && \
    apt-get install -y --no-install-recommends openssh-server python2.7 python-pip && \
    apt-get clean

# Just for TEST. Please remove it for another use case.
RUN apt-get install -y wget

# Make directory for sshd
RUN mkdir /var/run/sshd

# Set root's password. We don't use it.
RUN echo 'root:root' | chpasswd

# Allow root access, but deny the access by password.
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config

# Copy public key.
COPY ./id_rsa.pub /root/.ssh/authorized_keys

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]