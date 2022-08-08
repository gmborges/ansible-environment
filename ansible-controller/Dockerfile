FROM ubuntu

LABEL maintainer="gabrielmurilob@gmail.com"

# Setting timezone
ENV TZ=America/Sao_Paulo
RUN ln -s /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Installing dependencies and services
RUN apt update && \
    apt install -y tzdata \
        python3 \
        python3-pip \
        openssh-server \
        iputils-ping \
        vim

# Installing Ansible on Ubuntu
RUN apt install software-properties-common -y && \
    add-apt-repository --yes --update ppa:ansible/ansible && \
    apt install ansible -y

# Create a specific ansible user
RUN useradd -m -s /bin/bash ansible && \
    # Para gerar o hash da senha para o arquivo /etc/shadow: "printf 'ansible@pass' | mkpasswd --stdin -m sha-256"
    echo 'ansible:$5$isgEWRkjJcNp4t$hmVJytDR/huLIjSXnIuANCQwuCGkgR/GzQ737SMZsC9' | chpasswd --encrypted

# Copy SSH keys to ansible user directory
WORKDIR /home/ansible
COPY ./ssh-keys ./.ssh
RUN chown -R ansible:ansible ./.ssh && \
    chmod -R 770 ./.ssh && \
    chmod 600 ./.ssh/id_rsa && \
    chmod 664 ./.ssh/id_rsa.pub

# Disable SSH key host checking for Ansible
RUN echo "[defaults]" >> /etc/ansible/ansible.cfg && \
    echo "host_key_checking = False" >> /etc/ansible/ansible.cfg

# Create Ansible directory layout
COPY ./projects ./projects
RUN chmod -R 770 ./projects && \
    chown -R ansible:ansible ./projects

ENTRYPOINT ["tail", "-f", "/dev/null"]