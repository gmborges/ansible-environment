FROM ubuntu

LABEL maintainer="gabrielmurilob@gmail.com"

# Setting timezone
ENV TZ=America/Sao_Paulo
RUN ln -s /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Installing openssh-server
RUN apt update && \
    apt install -y openssh-server \
    iputils-ping

# Create a specific ansible user
RUN useradd -m -s /bin/bash ansible

# Copy Ansible Controller's SSH Public Key to a
WORKDIR /home/ansible
COPY ./id_rsa.pub ./.ssh/authorized_keys
RUN chown -R ansible:ansible ./.ssh && \
    chmod -R 770 ./.ssh && \
    chmod 644 ./.ssh/authorized_keys

CMD ["bash"]