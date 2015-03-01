## XRDP based Docker images of various Linux desktop environments

The Dockerfiles contained in this project will build experimental images of various Linux desktop environments.

### Goals and features

The features are:

 - accessible via RDP (provided by xrdp)
 - sound redirection
 - folder redirection

### Flavours and status

As the images are experimental, see the status below:

 - desktop-precise-xrdp-unity2d (several issues)
 - desktop-precise-xrdp-xfce4 (should work)
 - desktop-trusty-xrdp-xfce4 (should work)
 - desktop-vivid-xrdp-gnome3 (lots of issues)
 - desktop-wheezy-xrdp-xfce4 (no sound)

The second part of the name is the distributions codename.


### Quickstart

#### Managing the docker images

Provisioning via vagrant (building the images):

    vagrant up

    vagrant provision

    vagrant ssh

To run the image (e.g. Ubuntu 12.04 with Xfce4):

    docker run -i -p 3389:3389 -t desktop-precise-xrdp-xfce4

List unused containers:

    docker ps --no-trunc -aq

Get rid of unused containers:

    docker rm `docker ps --no-trunc -aq`


#### Login

Login with your RDP-client of choice:

    Host:     127.0.0.1
    Port:     3389

    Username: guest
    Password: docker
    Session:  sesman-xrdp

For testing sound redirection, start a terminal and run:

    paplay /usr/share/orage/sounds/Knock.wav
