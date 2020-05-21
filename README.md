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
# Question: Which of the following is an example of a container image? Linux, docker hub, container.jpg, nginx
## Answer: nginx
# Question: Which flag would you pass to specify a docker build referencing a file other than the default 'Dockerfile'?
## Answer: -f
The -f command is used to specify a dockerfile, with an alias of --file
# Question: Which Dockerfile stanza (Command) is best to use for changing the directory in a Dockerfile?
## Answer: WORKKIR
This is to change the current working directory in Dockerfiles.


