# How_create_huge_docker_images_centos7

The default Docker daemon on CentOS 7.x from the official package repository has two very annoying limitations when trying to create bigger images.

First is the default base image is 10gb space and second there is another barrier with the thin layer which is set to 100gb.

I needed a Docker image containing a big database which was about 120gb.

When I COPY'ed the huge database in the Dockerfile to build the image, I got a 

No space left on device

error when I was hitting the 10gb barrier. This can be controlled by the Docker Base device size which is set by default to
10gb and can be controlled by the dockerd daemon parameter dm.basesize.

After using a systemd drop-in config file with the changed dockerd parameter it went past this barrier but again after hitting 
a COPY amount of 100gb I got more weired errors:

ApplyLayer exit status 1 stdout:  stderr: input/output error

and
docker: Error response from daemon: devmapper: Thin Pool has 0 free data blocks which is less than minimum required

so I learned that I had to expand the thin layer. Sadly, you have to wipe your Docker data dir first :( in order to do so.

So here I provide the files and stuff I did in the end to make things work!

Please note that in this example I have changed my default Docker data dir to /data/docker-root from /var/lib/docker because of free space issues on my server!
