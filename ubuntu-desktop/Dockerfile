
#FROM ubuntu:jammy-20240911.1
FROM ubuntu:lunar-20230615

RUN apt update && DEBIAN_FRONTEND=noninteractive apt install lightdm -y && DEBIAN_FRONTEND=noninteractive apt-get install ubuntu-desktop -y locales sudo && apt install systemd -y

RUN apt update && \
    DEBIAN_FRONTEND=noninteractive apt install -y xrdp tigervnc-standalone-server && \
    adduser xrdp ssl-cert && \
    locale-gen en_US.UTF-8 && \
    update-locale LANG=en_US.UTF-8

ARG USER=testuser
ARG PASS=1234

RUN useradd -m $USER -p $(openssl passwd $PASS) && \
    usermod -aG sudo $USER && \
    chsh -s /bin/bash $USER

RUN echo "#!/bin/sh\n\
  export XDG_SESSION_DESKTOP=mate\n\
  export XDG_SESSION_TYPE=x11\n\
  export XDG_CURRENT_DESKTOP=MATE\n\
  export XDG_CONFIG_DIRS=/etc/xdg/xdg-mate:/etc/xdg\n\
  exec dbus-run-session -- mate-session" > /xstartup && chmod +x /xstartup

RUN cp -f /xstartup /etc/xrdp/startwm.sh \

RUN apt install xfce4 xfce4-goodies -y && apt install xrdp -y

EXPOSE 3389

CMD service dbus start; /usr/lib/systemd/systemd-logind & systemctl start xrdp; service slim start; bash
