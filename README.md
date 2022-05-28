*(Still working in progress, may not work yet)*

TODO
====
- Check guide from start to end
- Do not pass whole /dev/ to container, but use udev rules to pass specific devices

# docker-kiauh
Guide/helpers how to run Klipper/Moonraker/Mainsail/Fluidd in Docker container via kiuah (https://github.com/th33xitus/kiauh)

How does it work
================
It crates docker container with systemd & kiuah

Caution
=======
- Follow this guide only if you know what are you doing (scripts are short, you can read them)
- Used guide may not be the best from security point of view
- Executed container/deamon will keep restarting on failure (could consume lot of resources if you keep it restarting)

Guide
=====
1) Get dependencies on your machine
- bash, git (e.g. apt install bash git)
- docker (e.g. from https://get.docker.com/)
2) Clone repository & enter to the directory
```
git clone https://github.com/ybznek/docker-kiauh.git
cd docker-kiauh/
```
3) Check/modify config file

You may want to modify config file, especially "klipperConfigInRoot"
```
./config
```
4) Build docker image
```
./docker-kiuah build
```
5) Run docker container as daemon
```
./docker-kiuah runDaemon
```
6) Check status
```
./docker-kiuah status
```
There should be sth like
```
4ead22c9eee2   klipper   "/lib/systemd/systemd"   23 seconds ago   Up 22 seconds             printer
```
If you see sth like go to "Troubleshoot" section
```
b518f4e79feb   klipper   "/lib/systemd/systemd"   2 seconds ago   Restarting (255) Less than a second ago             printer
```
7) Execute kiuah & install what you want
```
./docker-kiuah kiuah
```
8) (Optional) open terminal (bash) in container
```
./docker-kiuah bash
```
 
Troubleshoot
============
If container keeps failing, you can use this command to execute to see output
```
./docker-kiuah run
```
Usually it keeps failing due to cgroups (you can to find some generic solution for "systemd in docker", it should help here)

- On Ubuntu x86_64 (standard desktop) add

systemd.unified_cgroup_hierarchy=0 to /etc/default/grub  && call sudo update-grub
e.g. :
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash systemd.unified_cgroup_hierarchy=0"
```

On RPI 4 aarch64 (64 bit OS)
add systemd.unified_cgroup_hierarchy=false to /boot/cmdline.txt
e.g. :
```
console=serial0,115200 console=tty1 root=PARTUUID=14b2f967-02 rootfstype=ext4 fsck.repair=yes rootwait systemd.unified_cgroup_hierarchy=false
```
Try to execute container in different way, check https://hub.docker.com/r/jrei/systemd-debian