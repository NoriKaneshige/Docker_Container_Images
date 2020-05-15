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
