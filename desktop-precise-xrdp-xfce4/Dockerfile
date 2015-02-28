FROM     ubuntu:precise
MAINTAINER "Stefan Jenkner <stefan@jenkner.org>"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update

RUN apt-get install -y --no-install-recommends \
        adduser \
        apt-utils \
        ca-certificates \
        devscripts \
        git-core \
        lsb-release \
        procps \
        wget \
    && apt-get clean

RUN cd /tmp \
    && git clone https://github.com/scarygliders/X11RDP-o-Matic.git \
    && cd X11RDP-o-Matic \
    && ./X11rdp-o-matic.sh --justdoit --withsound \
    && cd .. \
#    && rm -rf X11RDP-o-Matic
    && echo
RUN apt-get install -y --no-install-recommends \
    fakeroot \
    gnupg \
    ubuntu-keyring \
    pulseaudio \
    && apt-get clean
RUN apt-get -y build-dep pulseaudio
RUN cd /tmp && apt-get -y source pulseaudio && cd /tmp/pulseaudio-1.1 && dpkg-buildpackage -us -uc -rfakeroot
RUN cd /tmp/X11RDP-o-Matic/xrdp/sesman/chansrv/pulse \
    && make PULSE_DIR=/tmp/pulseaudio-1.1 \
    && mkdir -p /usr/lib/pulse-1.1/modules \
    && cp module-xrdp-sink.so /usr/lib/pulse-1.1/modules \
    && cp module-xrdp-source.so /usr/lib/pulse-1.1/modules \
    && rm -rf /tmp/pulseaudio-1.1

# Desktop Environment
RUN apt-get install -y --no-install-recommends \
        byobu \
        ca-certificates \
        command-not-found \
        dbus \
        firefox \
        fuse \
        language-pack-de-base \
        language-pack-en-base \
        notification-daemon \
        gstreamer0.10-pulseaudio \
        less \
        mr \
        paprefs \
        pulseaudio-utils \
        ssh-client \
        sudo \
        supervisor \
        tango-icon-theme \
        ttf-ubuntu-font-family \
        vcsh \
        vim-nox \
        wget \
        xfce4 \
        xfce4-artwork \
        xfce4-session \
        xfce4-terminal \
        xfonts-base \
        xterm \
        xubuntu-artwork \
        xubuntu-icon-theme \
        zsh \
    && apt-get clean

# Cleanup
RUN /etc/init.d/xrdp force-stop

# DBus
RUN mkdir -p /var/run/dbus
RUN chown messagebus:messagebus /var/run/dbus
RUN dbus-uuidgen --ensure

# Supervisord
RUN echo "[supervisord]\nnodaemon=true\n" > /etc/supervisor/conf.d/supervisord.conf
RUN echo "[program:xrdp]\ncommand=/usr/sbin/xrdp --nodaemon\n" >> /etc/supervisor/conf.d/supervisord.conf
RUN echo "[program:xrdp-sesman]\ncommand=/usr/sbin/xrdp-sesman -n\n" >> /etc/supervisor/conf.d/supervisord.conf
RUN echo "[program:dbus-daemon]\ncommand=/bin/dbus-daemon --system --nofork\n" >> /etc/supervisor/conf.d/supervisord.conf

# User
RUN useradd -m -d /home/guest -p guest guest
RUN echo 'guest:docker' |chpasswd
RUN chsh -s /bin/zsh guest
RUN adduser guest sudo
RUN adduser guest fuse
RUN echo "/usr/bin/startxfce4" > /home/guest/.xsession

EXPOSE 3389
CMD ["/usr/bin/supervisord"]
