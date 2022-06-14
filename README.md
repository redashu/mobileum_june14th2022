## training plan 

<img src="plan.png">

### labs 

<img src="lab.png">

### shortnames and other things 

<img src="sn.png">

### understanding problem in deploy /setup apps in bare-metal / hardware 

<img src="prob1.png">

### wastage of resources 

<img src="wst.png">

### bare-metal -- to Hypervisor -- {virtualization}---vm

<img src="vm.png">

### down side of using VM 

<img src="drop.png">

### Understanding Os components 

<img src="os.png">


## Introduction to containers 

<img src="cont1.png">

### VM vs Continers 

<img src="cont2.png">

### containers in more details 

<img src="cont3.png">

### Introduction to container runtimes -- intro to Docker 

<img src="docker.png">

### Docker ce installation on Linux VM -- aws cloud 

<img src="docker1.png">

### Docker architecture 

<img src="darch.png">

### connect docker client with your cred -- using ssh 

```
ssh  ashu@34.199.167.179
```

### docker client installed ??

```
docker  version 
Client:
 Version:           20.10.13
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 31 19:20:32 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
[ashu@docker-client ~]$ 

```

## setup docker host / server -- 

### step 1 installing 

```
yum install docker  
Failed to set locale, defaulting to C
Loaded plugins: extras_suggestions, langpacks, priorities, update-motd
amzn2-core                                                                                        | 3.7 kB  00:00:00     
Resolving Dependencies
--> Running transaction check
---> Package docker.x86_64 0:20.10.13-2.amzn2 will be installed
--> Processing Dependency: runc >= 1.0.0 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: libcgroup >= 0.40.rc1-5.15 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: containerd >= 1.3.2 for package: docker-20.10.13-2.amzn2.x86_64
--> Processing Dependency: pigz for package: docker-20.10.13-2.amzn2.x86_64
--> Running transaction check
---> Package containerd.x86_64 0:1.4.13-3.amzn2 will be installed
---> Package libcgroup.x86_64 0:0.41-21.amzn2 will be installed
---> Package pigz.x86_64 0:2.3.4-1.amzn2.0.1 will be installed
---> Package runc.x86_64 0:1.0.3-3.amzn2 will be installed
--> Finished Dependency Resolution


```

###  step 2 configure it to access remote connection 

```
cd /etc/sysconfig/
[root@ip-172-31-91-143 sysconfig]# ls
acpid       clock     docker          irqbalance  netconsole       raid-check     rpcbind    selinux
atd         console   docker-storage  keyboard    network          rdisc          rsyncd     sshd
authconfig  cpupower  i18n            man-db      network-scripts  readonly-root  rsyslog    sysstat
chronyd     crond     init            modules     nfs              rpc-rquotad    run-parts  sysstat.ioconf
[root@ip-172-31-91-143 sysconfig]# vim  docker
[root@ip-172-31-91-143 sysconfig]# cat  docker
# The max number of open files for the daemon itself, and all
# running containers.  The default value of 1048576 mirrors the value
# used by the systemd service unit.
DAEMON_MAXFILES=1048576

# Additional startup options for the Docker daemon, for example:
# OPTIONS="--ip-forward=true --iptables=true"
# By default we limit the number of open files per container
OPTIONS="--default-ulimit nofile=32768:65536 -H tcp://172.31.91.143:2375"

# How many seconds the sysvinit script waits for the pidfile to appear
# when starting the daemon.
DAEMON_PIDFILE_TIMEOUT=10

```

### starting docker engine service 

```
systemctl start  docker 
[root@ip-172-31-91-143 sysconfig]# systemctl status  docker 
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; disabled; vendor preset: disabled)
   Active: active (running) since Tue 2022-06-14 09:55:17 UTC; 14s ago
     Docs: https://docs.docker.com
  Process: 3183 ExecStartPre=/usr/libexec/docker/docker-setup-runtimes.sh (code=exited, status=0/SUCCESS)
  Process: 3181 ExecStartPre=/bin/mkdir -p /run/docker (code=exited, status=0/SUCCESS)
 Main PID: 3185 (dockerd)
    Tasks: 10
   Memory: 29.5M
   CGroup: /system.slice/docker.service
           └─3185 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --default-ulimit nofile=32768...

Jun 14 09:55:16 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:16.696316830Z" level=warning ms...ht"
Jun 14 09:55:16 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:16.696358567Z" level=warning ms...ce"
Jun 14 09:55:16 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:16.696824562Z" level=info msg="...t."
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.110760812Z" level=info msg="...ss"
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.220009510Z" level=info msg="...e."
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.239716597Z" level=info msg="....13
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.240237871Z" level=info msg="...on"
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal systemd[1]: Started Docker Application Container Engine.
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.296204742Z" level=info msg="...ck"
Jun 14 09:55:17 ip-172-31-91-143.ec2.internal dockerd[3185]: time="2022-06-14T09:55:17.303566647Z" level=info msg="...75"
Hint: Some lines were ellipsized, use -l to show in full.
[root@ip-172-31-91-143 sysconfig]# systemctl enable  docker 
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.

```

### ON Docker client side -- lets configure details of. docker server 

```
docker  images
Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?
[ashu@docker-client ~]$ 
[ashu@docker-client ~]$ docker context  ls
NAME        DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default *   Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
[ashu@docker-client ~]$ 
[ashu@docker-client ~]$ 
[ashu@docker-client ~]$ docker  context  create  remote-engine --docker  host="tcp://172.31.91.143:2375"
remote-engine
Successfully created context "remote-engine"
[ashu@docker-client ~]$ docker context  ls
NAME            DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default *       Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
remote-engine                                             tcp://172.31.91.143:2375 
```

### remote engine 

```
docker context  ls
NAME            DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default *       Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
remote-engine                                             tcp://172.31.91.143:2375                            
[ashu@docker-client ~]$ docker  context  use  remote-engine 
remote-engine
Current context is now "remote-engine"
[ashu@docker-client ~]$ docker context  ls
NAME              DESCRIPTION                               DOCKER ENDPOINT               KUBERNETES ENDPOINT   ORCHESTRATOR
default           Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                         swarm
remote-engine *                                             tcp://172.31.91.143:2375     
```





