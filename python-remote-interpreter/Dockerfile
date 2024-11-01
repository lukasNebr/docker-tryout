
FROM python:3.13

RUN apt update && apt install openssl -y

ARG USER=testuser
ARG PASS=1234

RUN useradd -m $USER -p $(openssl passwd $PASS) && \
    usermod -aG sudo $USER && \
    chsh -s /bin/bash $USER

COPY configs/ssh_config /etc/ssh/ssh_config
COPY configs/sshd_config /etc/ssh/sshd_config

RUN apt update && apt install openssh-server -y && service enable ssh && service start ssh

EXPOSE 22

CMD python3
