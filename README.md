# **Лекция №15-16: Технология контейнеризации. Введение в Docker. Docker контейнеры. Docker под капотом**
> _docker-1_
> _docker-2_

<details>
  <summary>Технология контейнеризации. Введение в Docker</summary>

## **Задание:**
Установка Docker, запуск контейнера на локальной машине, выполнение команд внутри контейнера, создание образа контейнера на основе запущенного.

Цель:
ПРИМЕЧАНИЕ: д/з будет необходимо выполнить после изучения 2-го занятия по docker - «Docker контейнеры. Docker под капотом»

В данном дз студент студент познакомится с контейнеризацией. Поймет ее отличие от виртуализации, узнает что такое Docker и зачем он нужен, создаст образ и запустит контейнер. В данном задании тренируются навыки: работы с Docker, создания образа, запуск контейнера.

Все действия описаны в методическом указании.

Критерии оценки:
0 б. - задание не выполнено 1 б. - задание выполнено 2 б. - выполнены все дополнительные задания
---

## **Выполнено:**
### **План:**
- Создание docker host
- Создание своего образа
- Работа с Docker Hub

Проверяем версии docker client и server
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker version
Client: Docker Engine - Community
 Version:           20.10.7
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        f0df350
 Built:             Wed Jun  2 11:56:24 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.7
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       b0f5bc3
  Built:            Wed Jun  2 11:54:48 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
~~~

Информация о текущем состоянии docker daemon
~~~bash
Deron-D_microservices git:(docker-2) ✗ docker info
Client:
Context:    default
Debug Mode: false
Plugins:
app: Docker App (Docker Inc., v0.9.1-beta3)
buildx: Build with BuildKit (Docker Inc., v0.5.1-docker)
scan: Docker Scan (Docker Inc., v0.8.0)

Server:
Containers: 3
Running: 0
Paused: 0
Stopped: 3
Images: 31
Server Version: 20.10.7
Storage Driver: overlay2
Backing Filesystem: xfs
Supports d_type: true
Native Overlay Diff: true
userxattr: false
Logging Driver: json-file
Cgroup Driver: cgroupfs
Cgroup Version: 1
Plugins:
Volume: local
Network: bridge host ipvlan macvlan null overlay
Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: inactive
Runtimes: io.containerd.runc.v2 io.containerd.runtime.v1.linux runc
Default Runtime: runc
Init Binary: docker-init
containerd version: e25210fe30a0a703442421b0f60afac609f950a3
runc version: v1.0.1-0-g4144b63
init version: de40ad0
Security Options:
seccomp
 Profile: default
Kernel Version: 4.18.0-240.15.1.el8_3.x86_64
Operating System: CentOS Linux 8
OSType: linux
Architecture: x86_64
CPUs: 12
Total Memory: 15.12GiB
Name: h470m
ID: JBP4:25C2:TZNQ:FRQ6:ILEH:NFQT:IYMH:F2CF:VFVZ:ZBKA:4L57:ANB7
Docker Root Dir: /var/lib/docker
~~~

Запустим первый контейнер:
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
b8dfde127a29: Pull complete
Digest: sha256:0fe98d7debd9049c50b597ef1f85b7c1e8cc81f59c8d623fcb2250e8bec85b38
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
~~~

#### Docker ps
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
➜  Deron-D_microservices git:(docker-2) ✗ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS                          PORTS     NAMES
1426faf3be74   hello-world              "/hello"                 About a minute ago   Exited (0) About a minute ago             interesting_babbage
~~~

#### Docker images
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker images
REPOSITORY                TAG       IMAGE ID       CREATED        SIZE
ubuntu                    xenial    38b3fa4640d4   2 weeks ago    135MB
ubuntu                    20.04     1318b700e415   2 weeks ago    72.8MB
ubuntu                    bionic    39a8cfeef173   2 weeks ago    63.1MB
httpd                     2.4       73b8cfec1155   3 weeks ago    138MB
debian                    stretch   2c3ad12c6ecf   3 weeks ago    101MB
hello-world               latest    d1165f221234   5 months ago   13.3kB
~~~

#### Docker run
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker run -it ubuntu:18.04 /bin/bash
Unable to find image 'ubuntu:18.04' locally
18.04: Pulling from library/ubuntu
Digest: sha256:7bd7a9ca99f868bf69c4b6212f64f2af8e243f97ba13abb3e641e03a7ceb59e8
Status: Downloaded newer image for ubuntu:18.04
root@c8e93bd68572:/# echo 'Hello world!' > /tmp/file
root@c8e93bd68572:/# exit
exit
➜  Deron-D_microservices git:(docker-2) ✗ docker run -it ubuntu:18.04 /bin/bash
root@305b369f5408:/# cat /tmp/file
cat: /tmp/file: No such file or directory
root@305b369f5408:/# exit
exit
~~~

#### Docker ps
Найдем ранее созданный контейнер в котором мы создали /tmp/file
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Names}}"
CONTAINER ID   IMAGE                    CREATED AT                      NAMES
305b369f5408   ubuntu:18.04             2021-08-16 22:21:01 +0300 MSK   brave_ramanujan
c8e93bd68572   ubuntu:18.04             2021-08-16 22:20:40 +0300 MSK   great_yonath
1426faf3be74   hello-world              2021-08-16 22:15:59 +0300 MSK   interesting_babbage
~~~

#### Docker start & attach
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker start c8e93bd68572
c8e93bd68572
➜  Deron-D_microservices git:(docker-2) ✗ docker attach c8e93bd68572
root@c8e93bd68572:/# cat /tmp/file
Hello world!

# Ctrl+p, Ctrl+q
root@c8e93bd68572:/# read escape sequence

➜  Deron-D_microservices git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED          STATUS         PORTS     NAMES
c8e93bd68572   ubuntu:18.04   "/bin/bash"   22 minutes ago   Up 2 minutes             great_yonath
~~~

#### Docker commit
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker commit c8e93bd68572 deron73/ubuntu-tmp-file
sha256:e42fe14c6b8eb05e75cbf030c4e490c622f9bdd6cd767487728722fff17790cb
~~~

#### Docker exec
~~~bash
Deron-D_microservices git:(docker-2) ✗ docker exec -it c8e93bd68572 bash
root@c8e93bd68572:/# ps axf
  PID TTY      STAT   TIME COMMAND
   12 pts/1    Ss     0:00 bash
   22 pts/1    R+     0:00  \_ ps axf
    1 pts/0    Ss+    0:00 /bin/bash
root@c8e93bd68572:/# exit
~~~

### Для сдачи домашнего задания
~~~bash
docker images | head -2 > docker-monolith/docker-1.log
~~~

### Задание со *
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗  docker inspect c8e93bd68572
[
    {
        "Id": "c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e",
        "Created": "2021-08-16T19:20:40.395358879Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 3284421,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-08-16T19:41:18.168971586Z",
            "FinishedAt": "2021-08-16T19:20:55.038367246Z"
        },
        "Image": "sha256:39a8cfeef17302cb7ce93cefe12368560fe62ef9d517808855f7bda79a1eb697",
        "ResolvConfPath": "/var/lib/docker/containers/c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e/hostname",
        "HostsPath": "/var/lib/docker/containers/c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e/hosts",
        "LogPath": "/var/lib/docker/containers/c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e/c8e93bd6857266e28632ba51d0f7610ba763584a6151c809e3f764f203d0176e-json.log",
        "Name": "/great_yonath",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f871233b84775e75b1c29e4db80e2922458df8f96894f1b3478636f968fb48b7-init/diff:/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/diff",
                "MergedDir": "/var/lib/docker/overlay2/f871233b84775e75b1c29e4db80e2922458df8f96894f1b3478636f968fb48b7/merged",
                "UpperDir": "/var/lib/docker/overlay2/f871233b84775e75b1c29e4db80e2922458df8f96894f1b3478636f968fb48b7/diff",
                "WorkDir": "/var/lib/docker/overlay2/f871233b84775e75b1c29e4db80e2922458df8f96894f1b3478636f968fb48b7/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "c8e93bd68572",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "ubuntu:18.04",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "fcf0e8d610cddd3dd0b541be3eafbbce4fb9bb8ce33f93220665febc24af7d1d",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/fcf0e8d610cd",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "6724b0abf122b0872f455199ad39de46d7ee9f3bb7928e42bc08788cead7306c",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6d2e447479c6b6e4e141a8281be413a46af2b28c9e182c69230e3ed653f2abcb",
                    "EndpointID": "6724b0abf122b0872f455199ad39de46d7ee9f3bb7928e42bc08788cead7306c",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

Deron-D_microservices git:(docker-2) ✗ docker inspect 39a8cfeef17302c
[
  {
      "Id": "sha256:39a8cfeef17302cb7ce93cefe12368560fe62ef9d517808855f7bda79a1eb697",
      "RepoTags": [
          "ubuntu:18.04",
          "ubuntu:bionic"
      ],
      "RepoDigests": [
          "ubuntu@sha256:7bd7a9ca99f868bf69c4b6212f64f2af8e243f97ba13abb3e641e03a7ceb59e8"
      ],
      "Parent": "",
      "Comment": "",
      "Created": "2021-07-26T21:21:31.071665434Z",
      "Container": "c92bfb9ad23f9f790e1b9aceecd94e4da4fd21892314d88c8baf1e767d826306",
      "ContainerConfig": {
          "Hostname": "c92bfb9ad23f",
          "Domainname": "",
          "User": "",
          "AttachStdin": false,
          "AttachStdout": false,
          "AttachStderr": false,
          "Tty": false,
          "OpenStdin": false,
          "StdinOnce": false,
          "Env": [
              "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
          ],
          "Cmd": [
              "/bin/sh",
              "-c",
              "#(nop) ",
              "CMD [\"bash\"]"
          ],
          "Image": "sha256:b8a5122daf391c9b0675a7d9b74c22896be683c3ee0935858ca8166d51756164",
          "Volumes": null,
          "WorkingDir": "",
          "Entrypoint": null,
          "OnBuild": null,
          "Labels": {}
      },
      "DockerVersion": "20.10.7",
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
              "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
          ],
          "Cmd": [
              "bash"
          ],
          "Image": "sha256:b8a5122daf391c9b0675a7d9b74c22896be683c3ee0935858ca8166d51756164",
          "Volumes": null,
          "WorkingDir": "",
          "Entrypoint": null,
          "OnBuild": null,
          "Labels": null
      },
      "Architecture": "amd64",
      "Os": "linux",
      "Size": 63137486,
      "VirtualSize": 63137486,
      "GraphDriver": {
          "Data": {
              "MergedDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/merged",
              "UpperDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/diff",
              "WorkDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/work"
          },
          "Name": "overlay2"
      },
      "RootFS": {
          "Type": "layers",
          "Layers": [
              "sha256:21639b09744fc39b4e1fe31c79cdf54470afe4d7239a517c4060bd181f8e3039"
          ]
      },
      "Metadata": {
          "LastTagTime": "0001-01-01T00:00:00Z"
      }
  }
]
~~~

#### Xем отличается контейнер от образа
Образ докера являются основой контейнеров. Образ - это упорядоченная коллекция изменений корневой файловой системы и соответствующих параметров выполнения для использования в среде выполнения контейнера. Образ обычно содержит объединение многоуровневых файловых систем, расположенных друг на друге. Образ не имеет состояния и никогда не изменяется.

Контейнер - это исполняемый (остановленный) экземпляр образа docker.

Контейнер Docker состоит из:
- Docker образа
- Среды выполнения
- Стандартного набора инструкций

Концепция заимствована из морских контейнеров, которые определяют стандарт для доставки товаров по всему миру. Docker определяет стандарт для отправки программного обеспечения.
(*) Взято из Docker Glossary
Хотелось бы еще добавить, что из одного образа можно запустить множество контейнеров.


#### Docker kill
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps -q
c8e93bd68572
➜  Deron-D_microservices git:(docker-2) ✗  docker kill $(docker ps -q)
c8e93bd68572
~~~

#### Docker system df
- Отображает сколько дискового пространства занято образами,
контейнерами и volume’ами
- Отображает сколько из них не используется и возможно удалить
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          10        5         3.035GB   509.2MB (16%)
Containers      6         0         12.99kB   12.99kB (100%)
Local Volumes   10        4         86.73MB   40.49MB (46%)
Build Cache     0         0         0B        0B
~~~

#### Docker rm & rmi
- rm удаляет контейнер, можно добавить флаг -f , чтобы удалялся
работающий container (будет послан SIGKILL )
- rmi удаляет image, если от него не зависят запущенные контейнеры

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker rm $(docker ps -a -q) # удалит все незапущенные контейнеры
305b369f5408
c8e93bd68572
1426faf3be74
75c7c3d1ff16
6547e0b1936d
5133ce0111ec
➜  Deron-D_microservices git:(docker-2) ✗  docker rmi $(docker images -q)
Untagged: deron73/ubuntu-tmp-file:latest
Deleted: sha256:e42fe14c6b8eb05e75cbf030c4e490c622f9bdd6cd767487728722fff17790cb
Deleted: sha256:e9e157101632d084907337bcf93e7ec684a8a068380203920c74409a9ea37ab
~~~

## **Полезное:**
- [Docker Docs. Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)
</details>
