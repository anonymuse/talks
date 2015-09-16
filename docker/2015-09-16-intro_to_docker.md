# [fit] Introduction to

# [fit] Docker

Jesse White
@jesse_white

---

# [fit] Me.
==============
==============
- Previously, sysadmin
- One 'devops', please
- Different problems
- Bigger problems

^ Background in help desk and traditional systems adminsitration. I heard about
this DevOps thing and thought that might yield a fatter paycheck. Once I
digested the words "cloud" and "devops" I mostly moved around in different
industries trying new problems, after which I tried finding bigger problems.

---

# [fit] You?

^ Show of hands for different types of users. Show of hands for different
levels of users.Any cooks (or other unusual types?)

---

# Where am I?

^ Thank Digital Ocean. Add organizers names here.
Why are we here today?

---

# [fit] Hack

^ We are here today to make some things. In short, to create. This is a fairly
overloaded term, so let us marinate for a moment on the definition.

---

# [fit] Hack
============
============
============

 /hak/
 _verb_
 1. to write computer programs for enjoyment

^ In popular culture hacking is largely talked about in terms of breaking into
things, but we'll stick with a more traditional sense that hacking is making
things, not breaking them.

---

# [fit] Hack on Docker

^ We are here today as we shared mutual interests, maybe we have some cool
ideas for a hackathon project, or maybe you want to get involved with submitted
code to open source projects. Docker Inc. has an incredible set of tutorials,
so let us run through some basics to get us all on the same page.

---

# [fit] Let's start by starting

^ Let us jump in and cover some basic Docker commands and operations.

---

# [fit] Docker
# [fit] check

^ We can make sure that the Docker binary is working as expected. Hopefully
you havee all installed Docker on your machines as a prerequisite of attending
this Hackathon, but I can help people after if they haveve had issues.

---

# Docker check
==============

```bash
$ sudo docker info
Containers: 4
Images: 59
Storage Driver: aufs
 Root Dir: /mnt/sda1/var/lib/docker/aufs
  Backing Filesystem: extfs
   Dirs: 67
    Dirperm1 Supported: true
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 4.0.9-boot2docker
    Operating System: Boot2Docker 1.8.1 (TCL 6.3); master : 7f12e95 - Thu Aug 13 03:24:56 UTC 2015
    CPUs: 8
    Total Memory: 1.955 GiB
    Name: boot2docker
    ID: DQEJ:WU5H:CJLP:KKSO:723C:CEGC:WBCP:4B7V:RW77:AILA:GPVY:I442
    Debug mode (server): true
    File Descriptors: 21
    Goroutines: 33
    System Time: 2015-09-16T02:10:42.676613791Z
    EventsListeners: 0
    Init SHA1:
    Init Path: /usr/local/bin/docker
    Docker Root Dir: /mnt/sda1/var/lib/docker
```

^ Here, we have ve passed the info command to the docker binary, which returns a
list of any containers, any images (the building blocks Docker uses to build
containers), the execution and storage drivers Docker is using, and its basic
configuration.  Docker has a client-server architecture. It has a single
binary, docker, that can act as both client and server. As a client, the docker
binary passes requests to the Docker daemon (e.g., asking it to return in-
formation about itself), and then processes those requests when they are
returned.

---

# [fit] Building our first
==========================
==========================
==========================
==========================
# [fit] container

---

# [fit] Docker run

---

# Docker run 1/2
====================
====================
====================
====================
====================

`$ sudo docker run -i -t ubuntu /bin/bash`

^A lot of things happened here, we can check each piece. First, we told Docker to run a command using docker run.

^We passed it two command line flags: -i, which keeps STDIN open from the container, and -t, which tells Docker to assign a pseudo-tty to the container.  This line is the base configuration needed to create a container with which we plan to interact on the command line ratherthan run as a daemonized service.

^Next, we told Docker to use the image ubuntu to create the container, provided by the Docker Hub Registry. which image to use to create a container, in this case the ubuntu image. You can use other similar base images (fedora, debian, centos, etc) as a basis for building your own images.

---


# Docker run 2/2
========================
========================

```bash
$ sudo docker run -i -t ubuntu /bin/bash
Pulling repository ubuntu from https://index.docker.io/v1 Pulling image 8
dbd9e392a964056420e5d58ca5cc376ef18e2de93b5cc90e868a1bbc8318c1c
(precise) from ubuntu Pulling 8
dbd9e392a964056420e5d58ca5cc376ef18e2de93b5cc90e868a1bbc8318c1c
metadata Pulling 8
dbd9e392a964056420e5d58ca5cc376ef18e2de93b5cc90e868a1bbc8318c1c
fs layer
Downloading 58337280/? (n/a) Pulling image
b750fe79269d2ec9a3c593ef05b4332b1d1a02a62b4accb2c21d589ff2f5f2dc↩ (quantal) from ubuntu
Pulling image 27cf784147099545 () from ubuntu root@fcd78e1a3569:/#
```


^ So what was happening in the background here? First, Docker checked locally for the ubuntu image. If it can not find the image on our local Docker host, it will reach out to the Docker Hub registry run by Docker, Inc., and look for it there.

^ Once Docker had found the image, it downloaded the image and stored it on the local host.  Docker then used this image to create a new container inside afilesystem. The container has a network, IP address, and a bridge interface to talk to the local host.

^ Finally, we told Docker which command to run in our new container, in this case launching a Bash shell with the /bin/bash command.

---

# [fit] The shell

---

# The container shell
=====================
=====================
=====================
=====================
=====================

`root@87dkjha01:/#`

^ When the container had been created, Docker ran the /bin/bash command inside
it; the containers shell was presented to us here.

---

# [fit] What
# [fit] do we have
# [fit] here?

^ We are logged into a new container, with a fun ID, as the root user. This is
a fully fledged Ubuntu host that we can interact with as if it were a bare
metal server or virtual machine at your preferred cloud provider.

---

# Check the hostname and hosts

```
root@87dkjha01:/# hostname
87dkjha01
root@87dkjha01:/# cat /etc/hosts
172.17.0.4 f7cbdac22a02
127.0.0.1 localhost
::1 localhost ip6-localhost ip6-loopback fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

^ We can see that our containers hostname is the container ID. Let us have a
look at the /etc/hosts file too. Docker has also added a host entry for our
container with its IP address.

^ Let's take a look at the hostname and /etc/hosts file.

---

# Check interfaces and processes

```
root@f87dkjha01:/# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
22: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:09 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.9/16 scope global eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:acff:fe11:9/64 scope link
       valid_lft forever preferred_lft forever
```

^ I am also expecting to see fairly standing networking information and a small
set of running processes.

---

# Check interfaces and processes
=========================
=========================
=========================
=========================

```
root@f87dkjha01:/# ps -aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1  18172  3308 ?        Ss   04:11   0:00 /bin/bash
root        16  0.0  0.1  15572  2168 ?        R+   04:14   0:00 ps -aux
```

^ From here you can do most anything that you do in a regular VM.
Install a package.
Make directories.
Etc.

---

# Out through the in door
=========================
=========================
=========================
=========================

```
root@67ff9e0bbf98:/# exit
docker@boot2docker:~$ sudo docker ps -a
CONTAINER ID  IMAGE     COMMAND       CREATED        STATUS                     PORTS   NAMES
67ff9e0bbf98  ubuntu    "/bin/bash"   8 seconds ago  Exited (0) 7 seconds ago           hungry_colden
```

^ Once we are finished with our container, we can type exit and return to the
command prompt of our host. What happened to the container?  Once we exited the
container, the only process that we had specified stopped, as did the container.
We can check for the stopped container with "docker ps -a". You can identify
containers through: short UUID, long UUID, and the name

---

# Container naming
==================

$ sudo docker run --name benji -i -t ubuntu /bin/bash
root@7e1743f23951:/#

^ Docker will automatically generate a name at random for each container we
create. We can see that the container we have just created is called gray_cat.
If we want to specify a particular container name in place of the automatically
generated name, we can do so using the --name flag.

^ We can use the container name in place of the container ID in most Docker
com- mands, as we'll see.  Container names are useful to help us identify and
build logical connections between containers and applications. It's also much
easier to remember a specific container name (e.g., web or db) than a container
ID or even a random name. I recommend using container names to make managing
your containers easier. Lastly, names much be unique.

---

# Starting a stopped container
==============================
==============================
==============================
```
root@7e1743f23951:/# exit
exit
docker@boot2docker:~$ sudo docker ps -l
CONTAINER ID        IMAGE   COMMAND       CREATED          STATUS                     PORTS  NAMES
7e1743f23951        ubuntu  "/bin/bash"   36 seconds ago   Exited (0) 6 seconds ago          benji
docker@boot2docker:~$ sudo docker start benji
benji
docker@boot2docker:~$ sudo docker ps
CONTAINER ID        IMAGE   COMMAND      CREATED              STATUS              PORTS     NAMES
7e1743f23951        ubuntu  "/bin/bash"  About a minute ago   Up 6 seconds                  benji
```
^ Run through starting a stopped container
Now, how do we attach back to the container?

---

# Attaching to a container
==========================
==========================
==========================
==========================
==========================

$ sudo docker attach benji
root@7e1743f23951:/#

^ For some reason, you may have to hit enter once to bring up the prompt.

---

# Creating daemonized containers v1
===============================
===============================
===============================

```
$ sudo docker run --name slimer -d ubuntu /bin/sh -c "while true; do echo hello \
world; sleep 1; done"
4cc473c016b19cf1b587403846625d6f682256d96f0854fb75387e74d717a819
```

^ In addition to these interactive containers, we can create longer-running
containers. Daemonized containers do not have the interactive session we have
just used and are ideal for running applications and services. Most your
containers will be daemonized.

^ Here, we have used the docker run command with the -d flag to tell Docker to
detach the container to the background.  We have also specified a while loop as
our container command. With this combination of flags, you will see that,
instead of being attached to a shell like our last container, the docker run
command has instead returned a container ID and returned us to our command
prompt.

---

# Creating daemonized containers v2
===============================
===============================
===============================
===============================

```
$ sudo docker ps
CONTAINER ID IMAGE  COMMAND                  CREATED         STATUS         PORTS NAMES
4cc473c016b1 ubuntu "/bin/sh -c "while tr"   12 seconds ago  Up 11 seconds        slimer
```
^ Now if we run docker ps, we can see a running container.

---

# [fit] Peeking inside

---

# Peeking inside 1/5

```
$ sudo docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

  -f, --follow=false        Follow log output
  --help=false              Print usage
  --since=                  Show logs since timestamp
  -t, --timestamps=false    Show timestamps
  --tail=all                Number of lines to show from the end of the logs
```

^ In order to see what is going on inside a running container, there are at
least 4 ways that I can think of to call the conatiner with the appropriate
command. Can someone give me the first example of what we could put into the
"CONTAINER?' field?

---

# Peeking inside 2/5
================
================
================
================
================
================

`$ sudo docker logs <NAME>`

---

# Peeking inside 3/5
================
================
================
================
================

```
$ sudo docker logs <NAME>
$ sudo docker logs <SHORT_UUID>
```

---

# Peeking inside 4/5
================
================
================
================

```
$ sudo docker logs <NAME>
$ sudo docker logs <SHORT_UUID>
$ sudo docker logs <LONG_UUID>
```

---

# Peeking inside 5/5
================
================
================

```
$ sudo docker logs <NAME>
$ sudo docker logs <SHORT_UUID>
$ sudo docker logs <LONG_UUID>
$ sudo docker `sudo docker ps -lq`
```

---

# [fit] Other
# [fit] tools

---

# Other tools
================
================
================
================
================
================

`$ sudo docker top slimer`


^ Based on this paradigm, you can leverage a few other command line tools.

---

# Other tools
================
================
================

```
$ sudo docker top slimer
$ sudo docker stop slimer
$ sudo docker inspect slimer
$ sudo docker rm slimer
```
