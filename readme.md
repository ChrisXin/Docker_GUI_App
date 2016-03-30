# overview

The graphical user interface on Docker Container can provided by software called the X Window System and VNCserver,
they define a device independent way of dealing with screens, keyboards and pointer devices; they also define a network protocol
for communication, and any program that knows how to "speak" this protocol can use it.

## Prerequisites:
i) docker base images for RHEL7 and SLES12

ii) about 600MB of local disk space and an Internet connection

iii) install stable version of docker

iv) install  [VNC viewer for windows](https://www.realvnc.com/download/viewer/)    

## Start Container

Running Docker Container in a specified port 590X (X could be picked from 1 to 9), this port will be used to listen as VNCserver

For SLES12 and RHEL7:

```
 docker run -it -p 590X:590X base_image_name /bin/bash
```

## Build dependencies in Docker Containers

For docker container in RHEL image

```
yum install tigervnc.s390x xorg-x11-server-Xvfb.s390x xorg-x11-xauth.s390x xterm.s390x
```

For docker container in SLES image:

```
zypper install  xorg-x11-Xvnc xauth tigervnc xkbcomp
```

## Start VNC server in docker container

X is the port number digit that is being specified in 590X.
This will start the VNC server with a display X and a virtual screen(monitor) 0.

For SLES:

```
vncserver :X
```

You usually will be asked for setting the password for the first time usage, but if you forget the password, you can always reset it

```
vncpasswd ˜/.vnc/passwd
```

Check vnc server list, make sure specific virtual screen is open.

```
vncserver -list
```

## Configure Host's firewall setting

```
sudo iptables -I INPUT -m state --state NEW -p tcp --destination-port 590X -j ACCEPT
```

## Start VNC viewer in Windows Desktop

Connect VNCviewer to the Host ip with port such as 168.0.0.0:X


## Testing GUI with Xclock

Open X terminal and run Xlock:

For RHEL:

```
yum install xclock
```

```
xclock
```

For SLES:

```
zypper install xclock
```

```
xclock
```

This will start Xclock GUI application.



## Appendix Dockerfile

For RHEL7
```
# Docker Gui App on VNC server
# Based on SLES 12 Image

from    rhel:latest

# Install vnc, xvfb in order to create a 'fake' display and firefox
run     yum install tigervnc.s390x xorg-x11-server-Xvfb.s390x xorg-x11-xauth.s390x xterm.s390x
run     vncserver :x
# Setup a password
run     echo MYVNCPASSWORD | vncpasswd -f > ~/.vnc/passwd
```

For SLES12
```
# Docker Gui App on VNC server
# Based on SLES 12 Image

from    sles:12

# Install vnc, xvfb in order to create a 'fake' display and firefox
run     zypper install  xorg-x11-Xvnc xauth tigervnc xkbcomp
run     vncserver :x
# Setup a password
run     echo MYVNCPASSWORD | vncpasswd -f > ~/.vnc/passwd
```
