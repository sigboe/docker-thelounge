[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://thelounge.github.io/
[hub]: https://hub.docker.com/r/linuxserver/thelounge/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# linuxserver/thelounge
[![](https://images.microbadger.com/badges/version/linuxserver/thelounge.svg)](https://microbadger.com/images/linuxserver/thelounge "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/linuxserver/thelounge.svg)](http://microbadger.com/images/linuxserver/thelounge "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/linuxserver/thelounge.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/linuxserver/thelounge.svg)][hub][![Build Status](http://jenkins.linuxserver.io:8080/buildStatus/icon?job=Dockers/LinuxServer.io/linuxserver-thelounge)](http://jenkins.linuxserver.io:8080/job/Dockers/job/LinuxServer.io/job/linuxserver-thelounge/)

TheLounge (a fork of shoutIRC) is a web IRC client that you host on your own server.

__What features does it have?__  
- Multiple user support
- Stays connected even when you close the browser
- Connect from multiple devices at once
- Responsive layout — works well on your smartphone
- _.. and more!_

[![theloungeirc](https://raw.githubusercontent.com/linuxserver/community-templates/master/lsiocommunity/img/shout-icon.png)][appurl]

## Usage

```
docker create \
  --name=thelounge \
  -v <path to data>:/config \
  -e PGID=<gid> -e PUID=<uid>  \
  -e TZ=<timezone> \
  -e USERS=no \
  -p 9000:9000 \
  linuxserver/thelounge
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 9000` - the port(s)
* `-v /config` -
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` for timezone information, eg Europe/London
* `-e USERS` set to yes to enable user accounts (See [Set up user accounts](#add-user-accounts))

It is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it thelounge /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application

To log in to the application, browse to https://<hostip>:9000.

### Add user accounts

If you want to add user accounts you must create the container with the `-e USERS` set to yes. See [Usage](#usage).

`docker exec -it thelounge node /app/node_modules/thelounge/index.js --home /config add <user>`

You will be asked to type a password. You can add more users by running the command again with a new username. Then reboot your container using `docker restart shout`

You should now be prompted for a password on the webinterface.


 
## Info

* Shell access whilst the container is running: `docker exec -it thelounge /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f thelounge`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' thelounge`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' linuxserver/thelounge`


## Versions

+ **06.02.17:** Rebase to alpine 3.5.
+ **14.10.16:** Bump to pickup 2.10 release.
+ **14.10.16:** Add version layer information.
+ **11.09.16:** Add layer badges to README.
+ **31.08.16:** Initial Release.
