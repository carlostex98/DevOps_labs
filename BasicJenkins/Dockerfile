FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

ARG USERNAME=devuser
ARG PXW=clr9090
ARG UID=1000
ARG GID=1000

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
      openssh-server sudo ca-certificates \
      vim nano curl wget git \
    && rm -rf /var/lib/apt/lists/*


RUN groupadd --gid ${GID} ${USERNAME} && \
    useradd  --uid ${UID} --gid ${GID} -m -s /bin/bash ${USERNAME} && \
    echo "${USERNAME}:${PXW}" | chpasswd


RUN usermod -aG sudo ${USERNAME} && \
    echo "%sudo ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-nopasswd


RUN mkdir -p /var/run/sshd && \
    sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
    sed -i 's/^#\?UsePAM.*/UsePAM yes/' /etc/ssh/sshd_config

EXPOSE 22

WORKDIR /home/${USERNAME}

# Arrancar el servidor SSH en primer plano
CMD ["/usr/sbin/sshd","-D","-e"]