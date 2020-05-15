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

