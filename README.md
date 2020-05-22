# Docker_Container_Images
# Container Images, Where To Find Them and How To Build Them
## The Mighty Hub: Using Docker Hub Registry Images
[Go to Repositories in docker site](http://hub.docker.com)

## start with official images, and later on change some to make your own images
## official images have great documentation
```
Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               latest              a0d4d95e478f        40 hours ago        541MB
httpd               latest              a8a9cbaadb0c        44 hours ago        166MB
centos              7                   b5b4d78bc90c        9 days ago          203MB
```
## we can download other versions too, alpine version is smaller
docker pull nginx:1.11.9
<br>
docker pull nginx:1.11
<br>
docker pull nginx:1.11.9-alpine

## Images and Their Layers: Discover the Image Cache
## images are designed using the union file system concept of making layers about changes
## The below is the history of image layers, every set of changes happens after the first bottom one
```
Koitaro@MacBook-Pro-3 ~ % docker history nginx:latest
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
602e111c06b6        3 weeks ago         /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  STOPSIGNAL SIGTERM           0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>           3 weeks ago         /bin/sh -c ln -sf /dev/stdout /var/log/nginx…   22B
<missing>           3 weeks ago         /bin/sh -c set -x     && addgroup --system -…   57.6MB
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV PKG_RELEASE=1~buster     0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV NJS_VERSION=0.3.9        0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  ENV NGINX_VERSION=1.17.10    0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>           3 weeks ago         /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>           3 weeks ago         /bin/sh -c #(nop) ADD file:9b8be2b52ee0fa31d…   69.2MB
```
## Look at the history of mysql, which is completely different set of image layers
```
Koitaro@MacBook-Pro-3 ~ % docker history mysql
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
a0d4d95e478f        41 hours ago        /bin/sh -c #(nop)  CMD ["mysqld"]               0B
<missing>           41 hours ago        /bin/sh -c #(nop)  EXPOSE 3306 33060            0B
<missing>           41 hours ago        /bin/sh -c #(nop)  ENTRYPOINT ["docker-entry…   0B
<missing>           41 hours ago        /bin/sh -c ln -s usr/local/bin/docker-entryp…   34B
<missing>           41 hours ago        /bin/sh -c #(nop) COPY file:b09451ebd8fb98d9…   13.2kB
<missing>           41 hours ago        /bin/sh -c #(nop) COPY dir:478f098f3681084f7…   1.22kB
<missing>           41 hours ago        /bin/sh -c #(nop)  VOLUME [/var/lib/mysql]      0B
<missing>           41 hours ago        /bin/sh -c {   echo mysql-community-server m…   405MB
<missing>           41 hours ago        /bin/sh -c echo "deb http://repo.mysql.com/a…   55B
<missing>           41 hours ago        /bin/sh -c #(nop)  ENV MYSQL_VERSION=8.0.20-…   0B
<missing>           41 hours ago        /bin/sh -c #(nop)  ENV MYSQL_MAJOR=8.0          0B
<missing>           41 hours ago        /bin/sh -c set -ex;  key='A4A9406876FCBD3C45…   2.61kB
<missing>           41 hours ago        /bin/sh -c apt-get update && apt-get install…   52.2MB
<missing>           41 hours ago        /bin/sh -c mkdir /docker-entrypoint-initdb.d    0B
<missing>           41 hours ago        /bin/sh -c set -eux;  savedAptMark="$(apt-ma…   4.17MB
<missing>           41 hours ago        /bin/sh -c #(nop)  ENV GOSU_VERSION=1.12        0B
<missing>           41 hours ago        /bin/sh -c apt-get update && apt-get install…   9.34MB
<missing>           41 hours ago        /bin/sh -c groupadd -r mysql && useradd -r -…   329kB
<missing>           47 hours ago        /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>           47 hours ago        /bin/sh -c #(nop) ADD file:ca8f84daab6688a5d…   69.2MB
```
## we never storing the same image data more than once in the file system
## we don't need to download the same layers that we already have
## container is just only a running process
![what_is_in_image](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/what_is_in_image.png)
![image_diaglam](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/image_diaglam.png)
![image_diaglam_2](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/image_diaglam_2.png)
![image_and_container_cow](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/image_and_container_cow.png)
![what_is_in_image_2](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/what_is_in_image_2.png)

## 'docker image inspect' returns JSON metadata about the image
## all the details about the image, image IDs, tags, which ports you need to open up inside of docker host if you want it to accept the connections, how images expect to be run
```
Koitaro@MacBook-Pro-3 ~ % docker image inspect nginx
[
    {
        "Id": "sha256:602e111c06b6934013578ad80554a074049c59441d9bcd963cb4a7feccede7a5",
        "RepoTags": [
            "nginx:latest"
        ],
        "RepoDigests": [
            "nginx@sha256:404ed8de56dd47adadadf9e2641b1ba6ad5ce69abf251421f91d7601a2808ebe",
            "nginx@sha256:f85c2305909e5881feec31efcf2a3449e5abd9b55a34522343c4b55ca2c947bb"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-04-23T13:03:01.355887897Z",
        "Container": "5d118015902f81257aaf639aadc130259632ebe86b1b4dd88f59b4f9a337f872",
        "ContainerConfig": {
            "Hostname": "5d118015902f",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.17.10",
                "NJS_VERSION=0.3.9",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"nginx\" \"-g\" \"daemon off;\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:627bb821c987e3d016d986ed9ca84ce3d0f555c344ca556aaea8cfca977615db",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGTERM"
        },
        "DockerVersion": "18.09.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.17.10",
                "NJS_VERSION=0.3.9",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:627bb821c987e3d016d986ed9ca84ce3d0f555c344ca556aaea8cfca977615db",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGTERM"
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 126774599,
        "VirtualSize": 126774599,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/d5d8301782beba07c5204ef68909df183c5dccdee304180e7652169e19fbadf0/diff:/var/lib/docker/overlay2/2eb8acd308a13b0259762aeda1897e79c72a1f5175b0244f5cccddc1a7dcbfe3/diff",
                "MergedDir": "/var/lib/docker/overlay2/93e7578f455a4aef050a3051ac6b78ea6c74147e5a75f9a7a0fa0a29984a247b/merged",
                "UpperDir": "/var/lib/docker/overlay2/93e7578f455a4aef050a3051ac6b78ea6c74147e5a75f9a7a0fa0a29984a247b/diff",
                "WorkDir": "/var/lib/docker/overlay2/93e7578f455a4aef050a3051ac6b78ea6c74147e5a75f9a7a0fa0a29984a247b/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:c2adabaecedbda0af72b153c6499a0555f3a769d52370469d8f6bd6328af9b13",
                "sha256:216cf33c0a2877e88bd687ced2d05331f442b8490962469220a3a63bf2aad3b0",
                "sha256:b3003aac411c1d650bc4e3757ad96afe8f98a99b81c4e760e09c6542ee674289"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```
## Image Tagging and Pushing to Docker Hub
## Tags are new lables for the same image ID
```
Koitaro@MacBook-Pro-3 ~ % docker pull mysql/mysql-server
Using default tag: latest
latest: Pulling from mysql/mysql-server
0e690826fc6e: Pull complete
0e6c49086d52: Pull complete
862ba7a26325: Pull complete
7731c802ed08: Pull complete
Digest: sha256:a82ff720911b2fd40a425fd7141f75d7c68fb9815ec3e5a5a881a8eb49677087
Status: Downloaded newer image for mysql/mysql-server:latest
docker.io/mysql/mysql-server:latest

Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
mysql/mysql-server   latest              716286be47c6        2 weeks ago         381MB

Koitaro@MacBook-Pro-3 ~ % docker pull nginx:mainline
mainline: Pulling from library/nginx
Digest: sha256:f938e7389670418efee4d5a8748600d9ec705170957774d25d4ec73bdbe004e2
Status: Image is up to date for nginx:mainline
docker.io/library/nginx:mainline

Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY           TAG                 IMAGE ID            CREATED             SIZE
nginx                mainline            9beeba249f3e        About an hour ago   127MB
mysql                latest              a0d4d95e478f        42 hours ago        541MB
httpd                latest              a8a9cbaadb0c        46 hours ago        166MB
```
## we can also re-tag existing image
## we have a new tag in an existing image. This doesn't exist in my docker hub yet.
```
Koitaro@MacBook-Pro-3 ~ % docker image tag --help

Usage:	docker image tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Koitaro@MacBook-Pro-3 ~ % docker image tag nginx norinori400/nginx

Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
nginx                             stable-perl         cf5662855280        6 days ago          178MB
nginx                             latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 latest              9beeba249f3e        6 days ago          127MB
mysql/mysql-server                latest              716286be47c6        3 weeks ago         381MB
nginx                             1.17.10-alpine      89ec9da68213        3 weeks ago         19.9MB
norinori400/norikaneshige/nginx   latest              602e111c06b6        4 weeks ago         127MB

Koitaro@MacBook-Pro-3 ~ % docker push norinori400/nginx
The push refers to repository [docker.io/norinori400/nginx]
6c7de695ede3: Preparing
2f4accd375d9: Preparing
ffc9b21953f4: Preparing
denied: requested access to the resource is denied                latest              602e111c06b6        3 weeks ago         127MB
```
## let's try to push
```
Koitaro@MacBook-Pro-3 ~ % docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: norinori400
Password:
Login Succeeded
Koitaro@MacBook-Pro-3 ~ % cat .docker/config.json
{
	"auths": {
		"https://index.docker.io/v1/": {}
	},
	"HttpHeaders": {
		"User-Agent": "Docker-Client/19.03.8 (darwin)"
	},
	"credsStore": "desktop",
	"stackOrchestrator": "swarm"
}%                                                                                                                       Koitaro@MacBook-Pro-3 ~ % docker push norinori400/nginx
The push refers to repository [docker.io/norinori400/nginx]
6c7de695ede3: Mounted from library/nginx
2f4accd375d9: Mounted from library/nginx
ffc9b21953f4: Mounted from library/nginx
latest: digest: sha256:8269a7352a7dad1f8b3dc83284f195bac72027dd50279422d363d49311ab7d9b size: 948
```
![new_image_pused_to_docker_hub](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/new_image_pused_to_docker_hub.png)

## Could give it another tag to the same image I just pushed to the docker hub
```
Koitaro@MacBook-Pro-3 ~ % docker image tag norinori400/nginx norinori400/nginx:testing
Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
nginx                             stable-perl         cf5662855280        6 days ago          178MB
nginx                             latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 testing             9beeba249f3e        6 days ago          127MB
mysql/mysql-server                latest              716286be47c6        3 weeks ago         381MB
nginx                             1.17.10-alpine      89ec9da68213        3 weeks ago         19.9MB
norinori400/norikaneshige/nginx   latest              602e111c06b6        4 weeks ago         127MB

Koitaro@MacBook-Pro-3 ~ % docker push norinori400/nginx:testing
The push refers to repository [docker.io/norinori400/nginx]
6c7de695ede3: Layer already exists
2f4accd375d9: Layer already exists
ffc9b21953f4: Layer already exists
testing: digest: sha256:8269a7352a7dad1f8b3dc83284f195bac72027dd50279422d363d49311ab7d9b size: 948

Koitaro@MacBook-Pro-3 ~ % docker logout
Removing login credentials for https://index.docker.io/v1/
```
![new_tag_to_the_same_image_and_pushed](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/new_tag_to_the_same_image_and_pushed.png)
## Building Images: The Dockerfile Basics
## Below is the basic dockerfile. The instruction of how we build the image
```
# NOTE: this example is taken from the default Dockerfile for the official nginx Docker Hub Repo
# https://hub.docker.com/_/nginx/
# NOTE: This file is slightly different than the video, because nginx versions have been updated 
#       to match the latest standards from docker hub... but it's doing the same thing as the video
#       describes
FROM debian:stretch-slim
# all images must have a FROM
# usually from a minimal Linux distribution like debian or (even better) alpine
# if you truly want to start with an empty container, use FROM scratch

ENV NGINX_VERSION 1.13.6-1~stretch
ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
# optional environment variable that's used in later lines and set as envvar when container is running

RUN apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
	&& \
	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
	found=''; \
	for server in \
		ha.pool.sks-keyservers.net \
		hkp://keyserver.ubuntu.com:80 \
		hkp://p80.pool.sks-keyservers.net:80 \
		pgp.mit.edu \
	; do \
		echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
	done; \
	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \
	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \
	&& apt-get update \
	&& apt-get install --no-install-recommends --no-install-suggests -y \
						nginx=${NGINX_VERSION} \
						nginx-module-xslt=${NGINX_VERSION} \
						nginx-module-geoip=${NGINX_VERSION} \
						nginx-module-image-filter=${NGINX_VERSION} \
						nginx-module-njs=${NJS_VERSION} \
						gettext-base \
	&& rm -rf /var/lib/apt/lists/*
# optional commands to run at shell inside container at build time
# this one adds package repo for nginx from nginx.org and installs it

RUN ln -sf /dev/stdout /var/log/nginx/access.log \
	&& ln -sf /dev/stderr /var/log/nginx/error.log
# forward request and error logs to docker log collector

EXPOSE 80 443
# expose these ports on the docker virtual network
# you still need to use -p or -P to open/forward these ports on host

CMD ["nginx", "-g", "daemon off;"]
# required: run this command when container is launched
# only one CMD allowed, so if there are multiple, last one wins
```
## ll is aliased to 'ls -alF'
```
Koitaro@MacBook-Pro-3 dockerfile-sample-1 % ls -alF
total 56
drwxr-xr-x   5 Koitaro  staff    160 May 21 21:28 ./
drwxr-xr-x  36 Koitaro  staff   1152 May 14 15:38 ../
-rw-r--r--   1 Koitaro  staff  12288 May 21 21:29 .Dockerfile.swo
-rw-r--r--   1 Koitaro  staff  12288 May 15 18:13 .Dockerfile.swp
-rw-r--r--@  1 Koitaro  staff   2471 May 21 21:09 Dockerfile
```
## Building Images: Running Docker Builds
## Docker file is an instraction of how to build the image.
## FROM command, when we build this image, actuallu pulls debian image from 
## docker hub, down to my local cache, execute line by line inside of my docker
## engine, and cache each of those layers.
## '.' means that 'build a docker file in this directory'
```
Koitaro@MacBook-Pro-3 dockerfile-sample-1 % docker image build -t customnginx .
Sending build context to Docker daemon   29.7kB
Step 1/7 : FROM debian:stretch-slim
stretch-slim: Pulling from library/debian
e62d08fa1eb1: Pull complete
Digest: sha256:b385ea429b383b690c955043d79050d1cb76346fbca67e3ed3649d5019dd6749
Status: Downloaded newer image for debian:stretch-slim
 ---> fa41698012c7
Step 2/7 : ENV NGINX_VERSION 1.13.6-1~stretch
 ---> Running in 70281a66e868
Removing intermediate container 70281a66e868
 ---> cf9a9e06cc60
Step 3/7 : ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
 ---> Running in 049489b1fb6a
Removing intermediate container 049489b1fb6a
 ---> 19be3e10f2c3
Step 4/7 : RUN apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 	&& 	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; 	found=''; 	for server in 	  	ha.pool.sks-keyservers.net 		hkp://keyserver.ubuntu.com:80 		hkp://p80.pool.sks-keyservers.net:80 		pgp.mit.edu 	; do 		echo "Fetching GPG key $NGINX_GPGKEY from $server"; 		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; 	done; 	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; 	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* 	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list 	&& apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y 	nginx=${NGINX_VERSION} 						nginx-module-xslt=${NGINX_VERSION} 						nginx-module-geoip=${NGINX_VERSION} 		   				nginx-module-image-filter=${NGINX_VERSION} 				    		nginx-module-njs=${NJS_VERSION} 						gettext-base 	&& rm -rf /var/lib/apt/lists/*
 ---> Running in 3f12285b9f14
Ign:1 http://deb.debian.org/debian stretch InRelease
Get:2 http://deb.debian.org/debian stretch-updates InRelease [91.0 kB]
Get:3 http://deb.debian.org/debian stretch Release [118 kB]
Get:4 http://deb.debian.org/debian stretch-updates/main amd64 Packages [27.9 kB]
Get:5 http://deb.debian.org/debian stretch Release.gpg [2410 B]
Get:6 http://deb.debian.org/debian stretch/main amd64 Packages [7083 kB]
Err:7 http://security.debian.org/debian-security stretch/updates InRelease
  Temporary failure resolving 'security.debian.org'
Fetched 7323 kB in 19s (366 kB/s)
Reading package lists...
W: Failed to fetch http://security.debian.org/debian-security/dists/stretch/updates/InRelease  Temporary failure resolving 'security.debian.org'
W: Some index files failed to download. They have been ignored, or old ones used instead.
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  libreadline7 readline-common
Suggested packages:
  libpcsclite1 parcimonie xloadimage | imagemagick | eog readline-doc
Recommended packages:
  gnupg1-curl gnupg1-l10n libldap-2.4-2
The following NEW packages will be installed:
  gnupg1 libreadline7 readline-common
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 822 kB of archives.
After this operation, 1935 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian stretch/main amd64 readline-common all 7.0-3 [70.4 kB]
Get:2 http://deb.debian.org/debian stretch/main amd64 libreadline7 amd64 7.0-3 [151 kB]
Get:3 http://deb.debian.org/debian stretch/main amd64 gnupg1 amd64 1.4.21-4+deb9u1 [601 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 822 kB in 0s (976 kB/s)
Selecting previously unselected package readline-common.
(Reading database ... 6317 files and directories currently installed.)
Preparing to unpack .../readline-common_7.0-3_all.deb ...
Unpacking readline-common (7.0-3) ...
Selecting previously unselected package libreadline7:amd64.
Preparing to unpack .../libreadline7_7.0-3_amd64.deb ...
Unpacking libreadline7:amd64 (7.0-3) ...
Selecting previously unselected package gnupg1.
Preparing to unpack .../gnupg1_1.4.21-4+deb9u1_amd64.deb ...
Unpacking gnupg1 (1.4.21-4+deb9u1) ...
Setting up readline-common (7.0-3) ...
Setting up libreadline7:amd64 (7.0-3) ...
Setting up gnupg1 (1.4.21-4+deb9u1) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
Fetching GPG key 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62 from ha.pool.sks-keyservers.net
Warning: apt-key output should not be parsed (stdout is not a terminal)
Executing: /tmp/apt-key-gpghome.IYOkbCC2xm/gpg.1.sh --keyserver ha.pool.sks-keyservers.net --keyserver-options timeout=10 --recv-keys 573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62
gpg: requesting key 7BD9BF62 from hkp server ha.pool.sks-keyservers.net
gpg: key 7BD9BF62: public key "nginx signing key <signing-key@nginx.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
Reading package lists...
Building dependency tree...
Reading state information...
The following packages were automatically installed and are no longer required:
  libreadline7 readline-common
Use 'apt autoremove' to remove them.
The following packages will be REMOVED:
  gnupg1*
0 upgraded, 0 newly installed, 1 to remove and 0 not upgraded.
After this operation, 1418 kB disk space will be freed.
(Reading database ... 6371 files and directories currently installed.)
Removing gnupg1 (1.4.21-4+deb9u1) ...
Reading package lists...
Building dependency tree...
Reading state information...
The following packages will be REMOVED:
  libreadline7* readline-common*
0 upgraded, 0 newly installed, 2 to remove and 0 not upgraded.
After this operation, 517 kB disk space will be freed.
(Reading database ... 6341 files and directories currently installed.)
Removing libreadline7:amd64 (7.0-3) ...
Removing readline-common (7.0-3) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
(Reading database ... 6317 files and directories currently installed.)
Purging configuration files for readline-common (7.0-3) ...
Ign:1 http://deb.debian.org/debian stretch InRelease
Get:2 http://deb.debian.org/debian stretch-updates InRelease [91.0 kB]
Get:3 http://deb.debian.org/debian stretch Release [118 kB]
Get:4 http://nginx.org/packages/mainline/debian stretch InRelease [2866 B]
Get:5 http://deb.debian.org/debian stretch Release.gpg [2410 B]
Get:6 http://deb.debian.org/debian stretch-updates/main amd64 Packages [27.9 kB]
Get:7 http://nginx.org/packages/mainline/debian stretch/nginx amd64 Packages [73.8 kB]
Get:8 http://deb.debian.org/debian stretch/main amd64 Packages [7083 kB]
Err:9 http://security.debian.org/debian-security stretch/updates InRelease
  Temporary failure resolving 'security.debian.org'
Fetched 7399 kB in 19s (370 kB/s)
Reading package lists...
W: Failed to fetch http://security.debian.org/debian-security/dists/stretch/updates/InRelease  Temporary failure resolving 'security.debian.org'
W: Some index files failed to download. They have been ignored, or old ones used instead.
Reading package lists...
Building dependency tree...
Reading state information...
The following additional packages will be installed:
  fontconfig-config fonts-dejavu-core libbsd0 libedit2 libexpat1
  libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu57 libjbig0
  libjpeg62-turbo libncurses5 libpng16-16 libssl1.1 libtiff5 libwebp6 libx11-6
  libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4 libxslt1.1 ucf
Suggested packages:
  libgd-tools geoip-bin
Recommended packages:
  geoip-database libgpm2 xml-core
The following NEW packages will be installed:
  fontconfig-config fonts-dejavu-core gettext-base libbsd0 libedit2 libexpat1
  libfontconfig1 libfreetype6 libgd3 libgeoip1 libicu57 libjbig0
  libjpeg62-turbo libncurses5 libpng16-16 libssl1.1 libtiff5 libwebp6 libx11-6
  libx11-data libxau6 libxcb1 libxdmcp6 libxml2 libxpm4 libxslt1.1 nginx
  nginx-module-geoip nginx-module-image-filter nginx-module-njs
  nginx-module-xslt ucf
0 upgraded, 32 newly installed, 0 to remove and 0 not upgraded.
Need to get 16.5 MB of archives.
After this operation, 55.3 MB of additional disk space will be used.
Get:1 http://deb.debian.org/debian stretch/main amd64 libxau6 amd64 1:1.0.8-1 [20.7 kB]
Get:2 http://deb.debian.org/debian stretch/main amd64 libssl1.1 amd64 1.1.0l-1~deb9u1 [1358 kB]
Get:3 http://nginx.org/packages/mainline/debian stretch/nginx amd64 nginx amd64 1.13.6-1~stretch [799 kB]
Get:4 http://deb.debian.org/debian stretch/main amd64 gettext-base amd64 0.19.8.1-2+deb9u1 [122 kB]
Get:5 http://deb.debian.org/debian stretch/main amd64 libbsd0 amd64 0.8.3-1 [83.0 kB]
Get:6 http://deb.debian.org/debian stretch/main amd64 libncurses5 amd64 6.0+20161126-1+deb9u2 [93.4 kB]
Get:7 http://deb.debian.org/debian stretch/main amd64 libedit2 amd64 3.1-20160903-3 [84.8 kB]
Get:8 http://nginx.org/packages/mainline/debian stretch/nginx amd64 nginx-module-geoip amd64 1.13.6-1~stretch [71.9 kB]
Get:9 http://nginx.org/packages/mainline/debian stretch/nginx amd64 nginx-module-image-filter amd64 1.13.6-1~stretch [75.9 kB]
Get:10 http://nginx.org/packages/mainline/debian stretch/nginx amd64 nginx-module-njs amd64 1.13.6.0.1.14-1~stretch [213 kB]
Get:11 http://deb.debian.org/debian stretch/main amd64 libicu57 amd64 57.1-6+deb9u3 [7705 kB]
Get:12 http://deb.debian.org/debian stretch/main amd64 libxml2 amd64 2.9.4+dfsg1-2.2+deb9u2 [920 kB]
Get:13 http://deb.debian.org/debian stretch/main amd64 ucf all 3.0036 [70.2 kB]
Get:14 http://deb.debian.org/debian stretch/main amd64 fonts-dejavu-core all 2.37-1 [1068 kB]
Get:15 http://nginx.org/packages/mainline/debian stretch/nginx amd64 nginx-module-xslt amd64 1.13.6-1~stretch [73.3 kB]
Get:16 http://deb.debian.org/debian stretch/main amd64 fontconfig-config all 2.11.0-6.7 [271 kB]
Get:17 http://deb.debian.org/debian stretch/main amd64 libexpat1 amd64 2.2.0-2+deb9u3 [83.7 kB]
Get:18 http://deb.debian.org/debian stretch/main amd64 libpng16-16 amd64 1.6.28-1+deb9u1 [280 kB]
Get:19 http://deb.debian.org/debian stretch/main amd64 libfreetype6 amd64 2.6.3-3.2+deb9u1 [438 kB]
Get:20 http://deb.debian.org/debian stretch/main amd64 libfontconfig1 amd64 2.11.0-6.7+b1 [331 kB]
Get:21 http://deb.debian.org/debian stretch/main amd64 libjpeg62-turbo amd64 1:1.5.1-2 [134 kB]
Get:22 http://deb.debian.org/debian stretch/main amd64 libjbig0 amd64 2.1-3.1+b2 [31.0 kB]
Get:23 http://deb.debian.org/debian stretch/main amd64 libtiff5 amd64 4.0.8-2+deb9u4 [238 kB]
Get:24 http://deb.debian.org/debian stretch/main amd64 libwebp6 amd64 0.5.2-1 [235 kB]
Get:25 http://deb.debian.org/debian stretch/main amd64 libxdmcp6 amd64 1:1.1.2-3 [26.3 kB]
Get:26 http://deb.debian.org/debian stretch/main amd64 libxcb1 amd64 1.12-1 [133 kB]
Get:27 http://deb.debian.org/debian stretch/main amd64 libx11-data all 2:1.6.4-3+deb9u1 [287 kB]
Get:28 http://deb.debian.org/debian stretch/main amd64 libx11-6 amd64 2:1.6.4-3+deb9u1 [748 kB]
Get:29 http://deb.debian.org/debian stretch/main amd64 libxpm4 amd64 1:3.5.12-1 [49.1 kB]
Get:30 http://deb.debian.org/debian stretch/main amd64 libgd3 amd64 2.2.4-2+deb9u5 [132 kB]
Get:31 http://deb.debian.org/debian stretch/main amd64 libgeoip1 amd64 1.6.9-4 [90.5 kB]
Get:32 http://deb.debian.org/debian stretch/main amd64 libxslt1.1 amd64 1.1.29-2.1+deb9u2 [233 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 16.5 MB in 20s (799 kB/s)
Selecting previously unselected package libxau6:amd64.
(Reading database ... 6317 files and directories currently installed.)
Preparing to unpack .../00-libxau6_1%3a1.0.8-1_amd64.deb ...
Unpacking libxau6:amd64 (1:1.0.8-1) ...
Selecting previously unselected package libssl1.1:amd64.
Preparing to unpack .../01-libssl1.1_1.1.0l-1~deb9u1_amd64.deb ...
Unpacking libssl1.1:amd64 (1.1.0l-1~deb9u1) ...
Selecting previously unselected package gettext-base.
Preparing to unpack .../02-gettext-base_0.19.8.1-2+deb9u1_amd64.deb ...
Unpacking gettext-base (0.19.8.1-2+deb9u1) ...
Selecting previously unselected package libbsd0:amd64.
Preparing to unpack .../03-libbsd0_0.8.3-1_amd64.deb ...
Unpacking libbsd0:amd64 (0.8.3-1) ...
Selecting previously unselected package libncurses5:amd64.
Preparing to unpack .../04-libncurses5_6.0+20161126-1+deb9u2_amd64.deb ...
Unpacking libncurses5:amd64 (6.0+20161126-1+deb9u2) ...
Selecting previously unselected package libedit2:amd64.
Preparing to unpack .../05-libedit2_3.1-20160903-3_amd64.deb ...
Unpacking libedit2:amd64 (3.1-20160903-3) ...
Selecting previously unselected package libicu57:amd64.
Preparing to unpack .../06-libicu57_57.1-6+deb9u3_amd64.deb ...
Unpacking libicu57:amd64 (57.1-6+deb9u3) ...
Selecting previously unselected package libxml2:amd64.
Preparing to unpack .../07-libxml2_2.9.4+dfsg1-2.2+deb9u2_amd64.deb ...
Unpacking libxml2:amd64 (2.9.4+dfsg1-2.2+deb9u2) ...
Selecting previously unselected package ucf.
Preparing to unpack .../08-ucf_3.0036_all.deb ...
Moving old data out of the way
Unpacking ucf (3.0036) ...
Selecting previously unselected package fonts-dejavu-core.
Preparing to unpack .../09-fonts-dejavu-core_2.37-1_all.deb ...
Unpacking fonts-dejavu-core (2.37-1) ...
Selecting previously unselected package fontconfig-config.
Preparing to unpack .../10-fontconfig-config_2.11.0-6.7_all.deb ...
Unpacking fontconfig-config (2.11.0-6.7) ...
Selecting previously unselected package libexpat1:amd64.
Preparing to unpack .../11-libexpat1_2.2.0-2+deb9u3_amd64.deb ...
Unpacking libexpat1:amd64 (2.2.0-2+deb9u3) ...
Selecting previously unselected package libpng16-16:amd64.
Preparing to unpack .../12-libpng16-16_1.6.28-1+deb9u1_amd64.deb ...
Unpacking libpng16-16:amd64 (1.6.28-1+deb9u1) ...
Selecting previously unselected package libfreetype6:amd64.
Preparing to unpack .../13-libfreetype6_2.6.3-3.2+deb9u1_amd64.deb ...
Unpacking libfreetype6:amd64 (2.6.3-3.2+deb9u1) ...
Selecting previously unselected package libfontconfig1:amd64.
Preparing to unpack .../14-libfontconfig1_2.11.0-6.7+b1_amd64.deb ...
Unpacking libfontconfig1:amd64 (2.11.0-6.7+b1) ...
Selecting previously unselected package libjpeg62-turbo:amd64.
Preparing to unpack .../15-libjpeg62-turbo_1%3a1.5.1-2_amd64.deb ...
Unpacking libjpeg62-turbo:amd64 (1:1.5.1-2) ...
Selecting previously unselected package libjbig0:amd64.
Preparing to unpack .../16-libjbig0_2.1-3.1+b2_amd64.deb ...
Unpacking libjbig0:amd64 (2.1-3.1+b2) ...
Selecting previously unselected package libtiff5:amd64.
Preparing to unpack .../17-libtiff5_4.0.8-2+deb9u4_amd64.deb ...
Unpacking libtiff5:amd64 (4.0.8-2+deb9u4) ...
Selecting previously unselected package libwebp6:amd64.
Preparing to unpack .../18-libwebp6_0.5.2-1_amd64.deb ...
Unpacking libwebp6:amd64 (0.5.2-1) ...
Selecting previously unselected package libxdmcp6:amd64.
Preparing to unpack .../19-libxdmcp6_1%3a1.1.2-3_amd64.deb ...
Unpacking libxdmcp6:amd64 (1:1.1.2-3) ...
Selecting previously unselected package libxcb1:amd64.
Preparing to unpack .../20-libxcb1_1.12-1_amd64.deb ...
Unpacking libxcb1:amd64 (1.12-1) ...
Selecting previously unselected package libx11-data.
Preparing to unpack .../21-libx11-data_2%3a1.6.4-3+deb9u1_all.deb ...
Unpacking libx11-data (2:1.6.4-3+deb9u1) ...
Selecting previously unselected package libx11-6:amd64.
Preparing to unpack .../22-libx11-6_2%3a1.6.4-3+deb9u1_amd64.deb ...
Unpacking libx11-6:amd64 (2:1.6.4-3+deb9u1) ...
Selecting previously unselected package libxpm4:amd64.
Preparing to unpack .../23-libxpm4_1%3a3.5.12-1_amd64.deb ...
Unpacking libxpm4:amd64 (1:3.5.12-1) ...
Selecting previously unselected package libgd3:amd64.
Preparing to unpack .../24-libgd3_2.2.4-2+deb9u5_amd64.deb ...
Unpacking libgd3:amd64 (2.2.4-2+deb9u5) ...
Selecting previously unselected package libgeoip1:amd64.
Preparing to unpack .../25-libgeoip1_1.6.9-4_amd64.deb ...
Unpacking libgeoip1:amd64 (1.6.9-4) ...
Selecting previously unselected package libxslt1.1:amd64.
Preparing to unpack .../26-libxslt1.1_1.1.29-2.1+deb9u2_amd64.deb ...
Unpacking libxslt1.1:amd64 (1.1.29-2.1+deb9u2) ...
Selecting previously unselected package nginx.
Preparing to unpack .../27-nginx_1.13.6-1~stretch_amd64.deb ...
----------------------------------------------------------------------

Thanks for using nginx!

Please find the official documentation for nginx here:
* http://nginx.org/en/docs/

Please subscribe to nginx-announce mailing list to get
the most important news about nginx:
* http://nginx.org/en/support.html

Commercial subscriptions for nginx are available on:
* http://nginx.com/products/

----------------------------------------------------------------------
Unpacking nginx (1.13.6-1~stretch) ...
Selecting previously unselected package nginx-module-geoip.
Preparing to unpack .../28-nginx-module-geoip_1.13.6-1~stretch_amd64.deb ...
Unpacking nginx-module-geoip (1.13.6-1~stretch) ...
Selecting previously unselected package nginx-module-image-filter.
Preparing to unpack .../29-nginx-module-image-filter_1.13.6-1~stretch_amd64.deb ...
Unpacking nginx-module-image-filter (1.13.6-1~stretch) ...
Selecting previously unselected package nginx-module-njs.
Preparing to unpack .../30-nginx-module-njs_1.13.6.0.1.14-1~stretch_amd64.deb ...
Unpacking nginx-module-njs (1.13.6.0.1.14-1~stretch) ...
Selecting previously unselected package nginx-module-xslt.
Preparing to unpack .../31-nginx-module-xslt_1.13.6-1~stretch_amd64.deb ...
Unpacking nginx-module-xslt (1.13.6-1~stretch) ...
Setting up libncurses5:amd64 (6.0+20161126-1+deb9u2) ...
Setting up libexpat1:amd64 (2.2.0-2+deb9u3) ...
Setting up libjpeg62-turbo:amd64 (1:1.5.1-2) ...
Setting up libpng16-16:amd64 (1.6.28-1+deb9u1) ...
Setting up libjbig0:amd64 (2.1-3.1+b2) ...
Setting up fonts-dejavu-core (2.37-1) ...
Setting up libtiff5:amd64 (4.0.8-2+deb9u4) ...
Setting up gettext-base (0.19.8.1-2+deb9u1) ...
Setting up libgeoip1:amd64 (1.6.9-4) ...
Setting up libicu57:amd64 (57.1-6+deb9u3) ...
Setting up libbsd0:amd64 (0.8.3-1) ...
Setting up ucf (3.0036) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up libxml2:amd64 (2.9.4+dfsg1-2.2+deb9u2) ...
Setting up libfreetype6:amd64 (2.6.3-3.2+deb9u1) ...
Setting up libxslt1.1:amd64 (1.1.29-2.1+deb9u2) ...
Processing triggers for libc-bin (2.24-11+deb9u4) ...
Setting up libssl1.1:amd64 (1.1.0l-1~deb9u1) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up libxdmcp6:amd64 (1:1.1.2-3) ...
Setting up libx11-data (2:1.6.4-3+deb9u1) ...
Setting up libxau6:amd64 (1:1.0.8-1) ...
Setting up libwebp6:amd64 (0.5.2-1) ...
Setting up libedit2:amd64 (3.1-20160903-3) ...
Setting up fontconfig-config (2.11.0-6.7) ...
debconf: unable to initialize frontend: Dialog
debconf: (TERM is not set, so the dialog frontend is not usable.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.24.1 /usr/local/share/perl/5.24.1 /usr/lib/x86_64-linux-gnu/perl5/5.24 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.24 /usr/share/perl/5.24 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base .) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up nginx (1.13.6-1~stretch) ...
invoke-rc.d: could not determine current runlevel
invoke-rc.d: policy-rc.d denied execution of start.
Setting up nginx-module-xslt (1.13.6-1~stretch) ...
----------------------------------------------------------------------

The xslt dynamic module for nginx has been installed.
To enable this module, add the following to /etc/nginx/nginx.conf
and reload nginx:

    load_module modules/ngx_http_xslt_filter_module.so;

Please refer to the module documentation for further details:
http://nginx.org/en/docs/http/ngx_http_xslt_module.html

----------------------------------------------------------------------
Setting up libxcb1:amd64 (1.12-1) ...
Setting up libfontconfig1:amd64 (2.11.0-6.7+b1) ...
Setting up nginx-module-njs (1.13.6.0.1.14-1~stretch) ...
----------------------------------------------------------------------

The nginScript dynamic modules for nginx have been installed.
To enable these modules, add the following to /etc/nginx/nginx.conf
and reload nginx:

    load_module modules/ngx_http_js_module.so;
    load_module modules/ngx_stream_js_module.so;

Please refer to the modules documentation for further details:
http://nginx.org/en/docs/njs_about.html
http://nginx.org/en/docs/http/ngx_http_js_module.html
http://nginx.org/en/docs/stream/ngx_stream_js_module.html

----------------------------------------------------------------------
Setting up libx11-6:amd64 (2:1.6.4-3+deb9u1) ...
Setting up nginx-module-geoip (1.13.6-1~stretch) ...
----------------------------------------------------------------------

The GeoIP dynamic modules for nginx have been installed.
To enable these modules, add the following to /etc/nginx/nginx.conf
and reload nginx:

    load_module modules/ngx_http_geoip_module.so;
    load_module modules/ngx_stream_geoip_module.so;

Please refer to the modules documentation for further details:
http://nginx.org/en/docs/http/ngx_http_geoip_module.html
http://nginx.org/en/docs/stream/ngx_stream_geoip_module.html

----------------------------------------------------------------------
Setting up libxpm4:amd64 (1:3.5.12-1) ...
Setting up libgd3:amd64 (2.2.4-2+deb9u5) ...
Setting up nginx-module-image-filter (1.13.6-1~stretch) ...
----------------------------------------------------------------------

The image filter dynamic module for nginx has been installed.
To enable this module, add the following to /etc/nginx/nginx.conf
and reload nginx:

    load_module modules/ngx_http_image_filter_module.so;

Please refer to the module documentation for further details:
http://nginx.org/en/docs/http/ngx_http_image_filter_module.html

----------------------------------------------------------------------
Processing triggers for libc-bin (2.24-11+deb9u4) ...
Removing intermediate container 3f12285b9f14
 ---> 61b31d825a22
Step 5/7 : RUN ln -sf /dev/stdout /var/log/nginx/access.log 	&& ln -sf /dev/stderr /var/log/nginx/error.log
 ---> Running in d72da4a00bcc
Removing intermediate container d72da4a00bcc
 ---> cd39d820a9f9
Step 6/7 : EXPOSE 80 443
 ---> Running in f6d246d6b256
Removing intermediate container f6d246d6b256
 ---> 2c1855d83b04
Step 7/7 : CMD ["nginx", "-g", "daemon off;"]
 ---> Running in b1c3d415aa85
Removing intermediate container b1c3d415aa85
 ---> b71c8c804777
Successfully built b71c8c804777
Successfully tagged customnginx:latest
```



```
Koitaro@MacBook-Pro-3 udemy-docker-mastery % cd dockerfile-sample-1
Koitaro@MacBook-Pro-3 dockerfile-sample-1 % docker image build -t customnginx .
...
Successfully built ebfa91266979
Successfully tagged customnginx:latest

Koitaro@MacBook-Pro-3 dockerfile-sample-1 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED              SIZE
customnginx                       latest              ebfa91266979        About

Koitaro@MacBook-Pro-3 dockerfile-sample-1 % docker image build -t customnginx .
Sending build context to Docker daemon   16.9kB
Step 1/7 : FROM debian:stretch-slim
 ---> fa41698012c7
Step 2/7 : ENV NGINX_VERSION 1.13.6-1~stretch
 ---> Using cache
 ---> 48aecbcb2e2d
Step 3/7 : ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
 ---> Using cache
 ---> 53cc711f4088
Step 4/7 : RUN apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 	&& 	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; 	found=''; 	for server in 		ha.pool.sks-keyservers.net 		hkp://keyserver.ubuntu.com:80 		hkp://p80.pool.sks-keyservers.net:80 		pgp.mit.edu 	; do 		echo "Fetching GPG key $NGINX_GPGKEY from $server"; 		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; 	done; 	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; 	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* 	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list 	&& apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y 	nginx=${NGINX_VERSION} 						nginx-module-xslt=${NGINX_VERSION} 						nginx-module-geoip=${NGINX_VERSION} 						nginx-module-image-filter=${NGINX_VERSION} 						nginx-module-njs=${NJS_VERSION} 						gettext-base 	&& rm -rf /var/lib/apt/lists/*
 ---> Using cache
 ---> d4e5fa6e6d5d
Step 5/7 : RUN ln -sf /dev/stdout /var/log/nginx/access.log 	&& ln -sf /dev/stderr /var/log/nginx/error.log
 ---> Using cache
 ---> 98df2ba614b7
Step 6/7 : EXPOSE 80 443
 ---> Using cache
 ---> 97839d41b3a6
Step 7/7 : CMD ["nginx", "-g", "daemon off;"]
 ---> Using cache
 ---> ebfa91266979
Successfully built ebfa91266979
Successfully tagged customnginx:latest
```
## Building the same Image: super quick because everything built already once
## Notice that it's using cache
```
Koitaro@MacBook-Pro-3 dockerfile-sample-1 % docker image build -t customnginx .
Sending build context to Docker daemon   29.7kB
Step 1/7 : FROM debian:stretch-slim
 ---> fa41698012c7
Step 2/7 : ENV NGINX_VERSION 1.13.6-1~stretch
 ---> Using cache
 ---> cf9a9e06cc60
Step 3/7 : ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
 ---> Using cache
 ---> 19be3e10f2c3
Step 4/7 : RUN apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y gnupg1 	&& 	NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; 	found=''; 	for server in 	ha.pool.sks-keyservers.net 		hkp://keyserver.ubuntu.com:80 		hkp://p80.pool.sks-keyservers.net:80 		pgp.mit.edu 	; do 		echo "Fetching GPG key $NGINX_GPGKEY from $server"; 		apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; 	done; 	test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; 	apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* 	&& echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list 	&& apt-get update 	&& apt-get install --no-install-recommends --no-install-suggests -y 						nginx=${NGINX_VERSION} 					nginx-module-xslt=${NGINX_VERSION} 						nginx-module-geoip=${NGINX_VERSION} 						nginx-module-image-filter=${NGINX_VERSION} 	nginx-module-njs=${NJS_VERSION} 						gettext-base 	&& rm -rf /var/lib/apt/lists/*
 ---> Using cache
 ---> 61b31d825a22
Step 5/7 : RUN ln -sf /dev/stdout /var/log/nginx/access.log 	&& ln -sf /dev/stderr /var/log/nginx/error.log
 ---> Using cache
 ---> cd39d820a9f9
Step 6/7 : EXPOSE 80 443
 ---> Using cache
 ---> 2c1855d83b04
Step 7/7 : CMD ["nginx", "-g", "daemon off;"]
 ---> Using cache
 ---> b71c8c804777
Successfully built b71c8c804777
Successfully tagged customnginx:latest
Koitaro@MacBook-Pro-3 dockerfile-sample-1 % ls -alF
total 56
drwxr-xr-x   5 Koitaro  staff    160 May 21 21:28 ./
drwxr-xr-x  36 Koitaro  staff   1152 May 14 15:38 ../
-rw-r--r--   1 Koitaro  staff  12288 May 21 21:29 .Dockerfile.swo
-rw-r--r--   1 Koitaro  staff  12288 May 15 18:13 .Dockerfile.swp
-rw-r--r--@  1 Koitaro  staff   2471 May 21 21:09 Dockerfile
```
## Building Images: Extending Official Images
## we goona copy inde.html into our container as we building it
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % ls -alF
total 16
drwxr-xr-x@  4 Koitaro  staff   128 May 15 19:34 ./
drwxr-xr-x  36 Koitaro  staff  1152 May 14 15:38 ../
-rw-r--r--   1 Koitaro  staff   410 May 14 15:31 Dockerfile
-rw-r--r--   1 Koitaro  staff   249 May 14 15:31 index.html

## here is the docker file
## this shows how we can extend/change an existing official image from Docker Hub
## we are using nginx as FROM command

FROM nginx:latest
# highly recommend you always pin versions for anything beyond dev/learn

WORKDIR /usr/share/nginx/html
# change working directory to root of nginx webhost
# using WORKDIR is preferred to using 'RUN cd /some/path'

COPY index.html index.html
# copy my source code from my local machine into your container images
# I don't have to specify EXPOSE or CMD because they're in my FROM
# we take a simple html file and overwrite the file in the nginx default derectory 

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                          PORTS               NAMES
e1ccc84f983e        nginx               "nginx -g 'daemon of…"   2 minutes ago       Exited (0) About a minute ago                       eager_pike
b55f96369007        nginx               "nginx -g 'daemon of…"   8 minutes ago       Exited (0) 8 minutes ago                            reverent_thompson
2d1404040941        602e111c06b6        "nginx -g 'daemon of…"   6 days ago          Exited (255) 5 days ago         80/tcp              my_nginx
6a1189201ac1        602e111c06b6        "nginx -g 'daemon of…"   6 days ago          Exited (255) 5 days ago         80/tcp              new_nginx
```
## check what the default nginx server does
## '--rm' means 'delete on quitting'
![what_default_nginx_server_does](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/what_default_nginx_server_does.png)
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container run -p 80:80 --rm nginx
172.17.0.1 - - [22/May/2020:03:32:21 +0000] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36" "-"
172.17.0.1 - - [22/May/2020:03:32:21 +0000] "GET /favicon.ico HTTP/1.1" 404 556 "http://localhost/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36" "-"
2020/05/22 03:32:21 [error] 8#8: *2 open() "/usr/share/nginx/html/favicon.ico" failed (2: No such file or directory), client: 172.17.0.1, server: localhost, request: "GET /favicon.ico HTTP/1.1", host: "localhost", referrer: "http://localhost/"
```
## Let's now buld our version of nginx image with our extra source code file, index.html
## we already have our nginx image cache, and just copying very small file and change in our working directory
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker image build -t nginx-with-html .
Sending build context to Docker daemon  3.072kB
Step 1/3 : FROM nginx:latest
 ---> 9beeba249f3e
Step 2/3 : WORKDIR /usr/share/nginx/html
 ---> Running in 32f05a379eee
Removing intermediate container 32f05a379eee
 ---> e7859fc8ce64
Step 3/3 : COPY index.html index.html
 ---> 31df27b07b71
Successfully built 31df27b07b71
Successfully tagged nginx-with-html:latest
```
## Then, this time, we just run the new image we just built, instead of default nginx server
## then, swith over to our browser, localhost
![run_the_new_image](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/run_the_new_image.png)

```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
nginx-with-html                   latest              31df27b07b71        3 minutes ago       127MB
customnginx                       latest              b71c8c804777        2 hours ago         108MB
nginx                             stable-perl         cf5662855280        6 days ago          178MB
nginx                             latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 testing             9beeba249f3e        6 days ago          127MB
debian                            stretch-slim        fa41698012c7        6 days ago          55.3MB
mysql/mysql-server                latest              716286be47c6        3 weeks ago         381MB
nginx                             1.17.10-alpine      89ec9da68213        3 weeks ago         19.9MB

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container run -p 80:80 --rm nginx-with-html
172.17.0.1 - - [22/May/2020:03:45:44 +0000] "GET / HTTP/1.1" 200 249 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.138 Safari/537.36" "-"
```
## Let's send this new image to docker hub
## We need to re-tag it first, then push it to docker hub
![push_newly_tagged_image](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/push_newly_tagged_image.png)
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker image tag nginx-with-html:latest norinori400/nginx-with-html:latest

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: norinori400
Password:
Login Succeeded

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
nginx-with-html                   latest              31df27b07b71        15 minutes ago      127MB
norinori400/nginx-with-html       latest              31df27b07b71        15 minutes ago      127MB
customnginx                       latest              b71c8c804777        2 hours ago         108MB
nginx                             stable-perl         cf5662855280        6 days ago          178MB
norinori400/nginx                 latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 testing             9beeba249f3e        6 days ago          127MB
nginx                             latest              9beeba249f3e        6 days ago          127MB
debian                            stretch-slim        fa41698012c7        6 days ago          55.3MB
mysql/mysql-server                latest              716286be47c6        3 weeks ago         381MB
nginx                             1.17.10-alpine      89ec9da68213        3 weeks ago         19.9MB
norinori400/norikaneshige/nginx   latest              602e111c06b6        4 weeks ago         127MB

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker push norinori400/nginx-with-html:latest
The push refers to repository [docker.io/norinori400/nginx-with-html]
4f24fa9ce38d: Pushed
6c7de695ede3: Mounted from norinori400/nginx
2f4accd375d9: Mounted from norinori400/nginx
ffc9b21953f4: Mounted from norinori400/nginx
latest: digest: sha256:fbe0a3d50d6df8664f21b59c98c8a6c55a42056653dd1217be24f64856c7cb7c size: 1155
```

# Question: Which of the following is an example of a container image? Linux, docker hub, container.jpg, nginx
## Answer: nginx
# Question: Which flag would you pass to specify a docker build referencing a file other than the default 'Dockerfile'?
## Answer: -f
The -f command is used to specify a dockerfile, with an alias of --file
# Question: Which Dockerfile stanza (Command) is best to use for changing the directory in a Dockerfile?
## Answer: WORKDIR
This is to change the current working directory in Dockerfiles.

# Build Your Own Image
![build_your_own_image](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/build_your_own_image.png)
```
Koitaro@MacBook-Pro-3 udemy-docker-mastery % cd dockerfile-assignment-1
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % ls
Dockerfile	app.js		package.json	routes
answer		bin		public		views
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % ls -alF
total 40
drwxr-xr-x  13 Koitaro  staff   416 May 14 15:31 ./
drwxr-xr-x  36 Koitaro  staff  1152 May 14 15:38 ../
-rw-r--r--   1 Koitaro  staff   402 May 14 15:31 .dockerignore
-rw-r--r--   1 Koitaro  staff   517 May 14 15:31 .gitignore
drwxr-xr-x   3 Koitaro  staff    96 May 14 15:31 .vscode/
-rw-r--r--   1 Koitaro  staff  2311 May 14 15:31 Dockerfile
drwxr-xr-x   3 Koitaro  staff    96 May 14 15:31 answer/
-rw-r--r--   1 Koitaro  staff  1256 May 14 15:31 app.js
drwxr-xr-x   3 Koitaro  staff    96 May 14 15:31 bin/
-rw-r--r--   1 Koitaro  staff   341 May 14 15:31 package.json
drwxr-xr-x   4 Koitaro  staff   128 May 14 15:31 public/
drwxr-xr-x   4 Koitaro  staff   128 May 14 15:31 routes/
drwxr-xr-x   5 Koitaro  staff   160 May 14 15:31 views/
```
## Dockerfile
```
# exposing 3000 here because app is designed to run on when we run it with docker run
# do not forget to use -p with port 80 going to port 3000
EXPOSE 3000

# - then it should use alpine package manager to install tini: 'apk add --update tini'
RUN apk add --update tini

# - then it should create directory /usr/src/app for app files with 'mkdir -p /usr/src/app'
RUN mkdir -p /usr/src/app

# - Node uses a "package manager", so it needs to copy in package.json file
# changing working directory
WORKDIR /usr/src/app
COPY package.json package.json

# - then it needs to run 'npm install' to install dependencies from that file
# - to keep it clean and small, run 'npm cache clean --force' after above
# usually when we run cleanup command, we want to run it in the same RUN line
# it removes everything at the same time, you only get one image layer
# && is a conditional, if the first command is successful, then do the second command
RUN npm install && npm cache clean

# - then it needs to copy in all files from current directory
# . . means copy everything from my current directory on the host to the current working directory in the image
COPY . .

# - then it needs to start container with command '/sbin/tini -- node ./bin/www'
CMD [ "tini", "--", "node", "./bin/www" ]

# - in the end you should be using FROM, RUN, WORKDIR, COPY, EXPOSE, and CMD commands

# Bonus Extra Credit
# this will not have you setting up a complete image useful for local development, test, and prod
# it's just meant to get you started with basic Dockerfile concepts and not focus too much on
# proper Node.js use in a container. **If you happen to be a Node.js Developer**, then 
# after you get through more of this course, you should come back and use my 
# Node Docker Good Defaults sample project on GitHub to change this Dockerfile for 
# better local development with more advanced topics
# https://github.com/BretFisher/node-docker-good-defaults
```
## Now let's build the image
## build it in that directory by "."
```
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker build -t testnode .
Sending build context to Docker daemon  443.9kB
Step 1/9 : FROM node:6-alpine
6-alpine: Pulling from library/node
bdf0201b3a05: Pull complete
e9fa13fdf0f5: Pull complete
ccc877228d8f: Pull complete
Digest: sha256:17258206fc9256633c7100006b1cfdf25b129b6a40b8e5d37c175026482c84e3
Status: Downloaded newer image for node:6-alpine
 ---> dfc29bfa7d41
Step 2/9 : EXPOSE 3000
 ---> Running in 32ab6c59494d
Removing intermediate container 32ab6c59494d
 ---> 2ba10fc433c5
Step 3/9 : RUN apk add --update tini
 ---> Running in c24036a4460b
fetch http://dl-cdn.alpinelinux.org/alpine/v3.9/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.9/community/x86_64/APKINDEX.tar.gz
(1/1) Installing tini (0.18.0-r0)
Executing busybox-1.29.3-r10.trigger
OK: 7 MiB in 17 packages
Removing intermediate container c24036a4460b
 ---> 1d96c808dbef
Step 4/9 : RUN mkdir -p /usr/src/app
 ---> Running in 83ee2d458fbd
Removing intermediate container 83ee2d458fbd
 ---> 88aa250fdab0
Step 5/9 : WORKDIR /usr/src/app
 ---> Running in b9849802c90b
Removing intermediate container b9849802c90b
 ---> 9929b57847d5
Step 6/9 : COPY package.json package.json
 ---> c1e5a373012d
Step 7/9 : RUN npm install && npm cache clean
 ---> Running in 94c888a5ad5f
dockerfile-assignment-1@0.0.0 /usr/src/app
+-- body-parser@1.19.0
| +-- bytes@3.1.0
| +-- content-type@1.0.4
| +-- depd@1.1.2
| +-- http-errors@1.7.2
| | +-- inherits@2.0.3
| | `-- toidentifier@1.0.0
| +-- iconv-lite@0.4.24
| | `-- safer-buffer@2.1.2
| +-- on-finished@2.3.0
| | `-- ee-first@1.1.1
| +-- qs@6.7.0
| +-- raw-body@2.4.0
| | `-- unpipe@1.0.0
| `-- type-is@1.6.18
|   +-- media-typer@0.3.0
|   `-- mime-types@2.1.27
|     `-- mime-db@1.44.0
+-- cookie-parser@1.4.5
| +-- cookie@0.4.0
| `-- cookie-signature@1.0.6
+-- debug@2.6.9
| `-- ms@2.0.0
+-- express@4.17.1
| +-- accepts@1.3.7
| | `-- negotiator@0.6.2
| +-- array-flatten@1.1.1
| +-- content-disposition@0.5.3
| +-- encodeurl@1.0.2
| +-- escape-html@1.0.3
| +-- etag@1.8.1
| +-- finalhandler@1.1.2
| +-- fresh@0.5.2
| +-- merge-descriptors@1.0.1
| +-- methods@1.1.2
| +-- parseurl@1.3.3
| +-- path-to-regexp@0.1.7
| +-- proxy-addr@2.0.6
| | +-- forwarded@0.1.2
| | `-- ipaddr.js@1.9.1
| +-- range-parser@1.2.1
| +-- safe-buffer@5.1.2
| +-- send@0.17.1
| | +-- destroy@1.0.4
| | +-- mime@1.6.0
| | `-- ms@2.1.1
| +-- serve-static@1.14.1
| +-- setprototypeof@1.1.1
| +-- statuses@1.5.0
| +-- utils-merge@1.0.1
| `-- vary@1.1.2
+-- hbs@4.0.6
| +-- handlebars@4.3.5
| | +-- neo-async@2.6.1
| | +-- optimist@0.6.1
| | | +-- minimist@0.0.10
| | | `-- wordwrap@0.0.3
| | +-- source-map@0.6.1
| | `-- uglify-js@3.9.3
| |   `-- commander@2.20.3
| `-- walk@2.3.14
|   `-- foreachasync@3.0.0
+-- morgan@1.10.0
| +-- basic-auth@2.0.1
| +-- depd@2.0.0
| `-- on-headers@1.0.2
`-- serve-favicon@2.5.0
  +-- ms@2.1.1
  `-- safe-buffer@5.1.1

Removing intermediate container 94c888a5ad5f
 ---> f1a02e55b2fc
Step 8/9 : COPY . .
 ---> 87b1cc0fa221
Step 9/9 : CMD [ "tini", "--", "node", "./bin/www" ]
 ---> Running in c046b41046ea
Removing intermediate container c046b41046ea
 ---> 6db78297cc20
Successfully built 6db78297cc20
Successfully tagged testnode:latest


Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
testnode                          latest              6db78297cc20        6 minutes ago       64.5MB
nginx-with-html                   latest              31df27b07b71        11 hours ago        127MB
norinori400/nginx-with-html       latest              31df27b07b71        11 hours ago        127MB
customnginx                       latest              b71c8c804777        13 hours ago        108MB
nginx                             stable-perl         cf5662855280        6 days ago          178MB
nginx                             latest              9beeba249f3e        6 days ago          127MB
norinori400/nginx                 latest              9beeba249f3e        6 days ago          127MB
```
## Now let's run a container
## open the port, wants to run 80, listens on 3000, specify the image name
![run_a_container_on_the_image](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/run_a_container_on_the_image.png)
```
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker container run --rm -p 80:3000 testnode
GET /favicon.ico 404 115.159 ms - 970
GET / 200 14.207 ms - 290
GET /stylesheets/style.css 200 14.463 ms - 111
GET /images/picard.gif 200 13.427 ms - 417700
```
## Need to rename the created image before we push it into docker hub
![pushed_to_docker_hub](https://github.com/NoriKaneshige/Docker_Container_Images/blob/master/pushed_to_docker_hub.png)
```
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
testnode                          latest              6db78297cc20        34 minutes ago      64.5MB
nginx-with-html                   latest              31df27b07b71        12 hours ago        127MB
norinori400/nginx-with-html       latest              31df27b07b71        12 hours ago        127MB

Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker tag --help

Usage:	docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]

Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE

Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker tag testnode norinori400/testing-node

Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: norinori400
Password:
Login Succeeded

Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker push norinori400/testing-node
The push refers to repository [docker.io/norinori400/testing-node]
25cf82c5e772: Pushed
74a38db8ad6d: Pushed
e7e6675cbcf8: Pushed
0b326fb814a5: Pushed
39aae098b757: Pushed
f168d52a989d: Mounted from library/node
17b7c23fba03: Mounted from library/node
a464c54f93a9: Mounted from library/node
latest: digest: sha256:0781b843fc0aef3b449faa8aefbbe770acf677b7d838f4363490b590029e1e61 size: 1997

Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker image ls
REPOSITORY                        TAG                 IMAGE ID            CREATED             SIZE
testnode                          latest              6db78297cc20        46 minutes ago      64.5MB
norinori400/testing-node          latest              6db78297cc20        46 minutes ago      64.5MB
nginx-with-html                   latest              31df27b07b71        12 hours ago        127MB
```
## Let's remove the image, norinori400/testing-node, and run new container by downloading norinori400/testing-node from docker hub
```
Koitaro@MacBook-Pro-3 dockerfile-assignment-1 % docker image rm norinori400/testing-node
Untagged: norinori400/testing-node:latest
Untagged: norinori400/testing-node@sha256:0781b843fc0aef3b449faa8aefbbe770acf677b7d838f4363490b590029e1e61



```
