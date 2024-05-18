## Laboratornaya rabota 8 ##


Данная лабораторная работа посвещена изучению систем автоматизации развёртывания и управления приложениями на примере Docker


`````sh
$ open https://docs.docker.com/get-started/
`````


1. Создаём Dockerfile

`````sh
cat >> Dockerfile <<EOF
FROM ubuntu:18.04

RUN apt update
RUN apt install -yy gcc g++ cmake

COPY . print/
WORKDIR print

RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
RUN cmake --build _build

ENV LOG_PATH /home/logs/log.txt

VOLUME /home/logs

WORKDIR _install/bin

ENTRYPOINT ./demo
EOF
`````

2. Создаём main.yml


`````sh
name: CMake

on:
 push:
  branches: [master]
 pull_request:
  branches: [master]

jobs: 
  build:
    runs-on: ubuntu-latest
  
    steps:
    - uses: actions/checkout@v3
  
    - name: Dockerki
      run: sudo apt install runc containerd docker.io
  
    - name: RunDocke
      run: sudo docker build -t logger .
`````

3. Workflow сработал, теперь проверим на локальном репозитории.





`````sh
sudo docker build -t logger .
sudo docker images
sudo docker inspect logger
`````

`````sh
[+] Building 10.9s (10/12)                                                                               docker:default
[+] Building 18.3s (13/13) FINISHED                                                                      docker:default
 => [internal] load .dockerignore                                                                                  0.3s
 => => transferring context: 2B                                                                                    0.0s
 => [internal] load build definition from Dockerfile                                                               0.5s
 => => transferring dockerfile: 338B                                                                               0.0s
 => [internal] load metadata for docker.io/library/ubuntu:18.04                                                    1.7s
 => [1/8] FROM docker.io/library/ubuntu:18.04@sha256:152dc042452c496007f07ca9127571cb9c29697f42acbfad72324b2bb2e4  0.0s
 => [internal] load build context                                                                                  0.3s
 => => transferring context: 25.00kB                                                                               0.0s
 => CACHED [2/8] RUN apt update                                                                                    0.0s
 => CACHED [3/8] RUN apt install -yy gcc g++ cmake                                                                 0.0s
 => [4/8] COPY . print/                                                                                            1.0s
 => [5/8] WORKDIR print                                                                                            1.0s
 => [6/8] RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install                        2.9s
 => [7/8] RUN cmake --build _build                                                                                 3.3s
 => [8/8] WORKDIR _install/bin                                                                                     1.2s
 => exporting to image                                                                                             4.7s
 => => exporting layers                                                                                            4.6s
 => => writing image sha256:43991684312d9c52b8eba1c3222cfaa20651fde2d98d39ac0caf2c5f875ed77c                       0.1s
 => => naming to docker.io/library/logger                                                                          0.1s

sudo docker images
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
logger       latest    43991684312d   29 seconds ago   336MB

sudo docker inspect logger

[
    {
        "Id": "sha256:43991684312d9c52b8eba1c3222cfaa20651fde2d98d39ac0caf2c5f875ed77c",
        "RepoTags": [
            "logger:latest"
        ],
        "RepoDigests": [],
        "Parent": "",
        "Comment": "buildkit.dockerfile.v0",
        "Created": "2024-05-18T19:21:28.077807036+03:00",
        "Container": "",
        "ContainerConfig": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": null,
            "Cmd": null,
            "Image": "",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "DockerVersion": "",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "LOG_PATH=/home/logs/log.txt"
            ],
            "Cmd": null,
            "Image": "",
            "Volumes": {
                "/home/logs": {}
            },
            "WorkingDir": "/print/_install/bin",
            "Entrypoint": [
                "/bin/sh",
                "-c",
                "./demo"
            ],
            "OnBuild": null,
            "Labels": {
                "org.opencontainers.image.ref.name": "ubuntu",
                "org.opencontainers.image.version": "18.04"
            }
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 336300662,
        "VirtualSize": 336300662,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/snap/docker/common/var-lib-docker/overlay2/8bnownbxeypagwcu9i1te6ibw/diff:/var/snap/docker/common/var-lib-docker/overlay2/20mrhfe4cc86kw1227643ki48/diff:/var/snap/docker/common/var-lib-docker/overlay2/bgkzd2sg671l3oioxypuvfcm5/diff:/var/snap/docker/common/var-lib-docker/overlay2/g7z7lwgaoe376niyv3jxdufxi/diff:/var/snap/docker/common/var-lib-docker/overlay2/a64pf90x1bzxd6y0sf3yl6qgy/diff:/var/snap/docker/common/var-lib-docker/overlay2/q8kac1huprfisxu3flq2lei9e/diff:/var/snap/docker/common/var-lib-docker/overlay2/f50b89b4399a9a1ede7b3798295e212654790226a5d437b0af721680d21e68e9/diff",
                "MergedDir": "/var/snap/docker/common/var-lib-docker/overlay2/o7tit1892n41x1ufnlx3g3xfr/merged",
                "UpperDir": "/var/snap/docker/common/var-lib-docker/overlay2/o7tit1892n41x1ufnlx3g3xfr/diff",
                "WorkDir": "/var/snap/docker/common/var-lib-docker/overlay2/o7tit1892n41x1ufnlx3g3xfr/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:548a79621a426b4eb077c926eabac5a8620c454fb230640253e1b44dc7dd7562",
                "sha256:61727ad9b0142c093056fe9352d0a8cd28c367ca12e49ae3588b7095e5ae71a4",
                "sha256:22061ba761ff05ae804f349286be533347f40a1eaf97678a75499d594532b3be",
                "sha256:faab39121d968e3b0f45a8d9dd6876f210bdff2611d9d0de93cdac74509d9d71",
                "sha256:5f70bf18a086007016e948b04aed3b82103a36bea41755b6cddfaf10ace3c6ef",
                "sha256:17ff3898a430b0dd8d53920578c295addbc79ab54d7ff3a5ef999076e7e383d5",
                "sha256:9bf162c33d585d62345c274b04c4bc416370cfd0104fba85d70ea10d3eb2c4ca",
                "sha256:c869d6a3076b72220a7a918e799b87265b6689281ffe42bbd7bea6041bdce5f8"
            ]
        },
        "Metadata": {
            "LastTagTime": "2024-05-18T19:21:33.021630909+03:00"
        }
    }
]

`````


