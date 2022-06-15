## training plan 

<img src="plan.png">

## python based docker image assignment 

### Dockerfile 

```
from alpine 
LABEL email=ashutoshh@linux.com 
RUN apk add python3 
RUN mkdir  /code 
ADD https://raw.githubusercontent.com/redashu/pythonLang/main/while.py /code/ 
# copy and add both are same but add support URL sources also 
ENTRYPOINT python3 /code/while.py 
# ENTrypoint is same as CMD 
```

### image build 

```
docker  build  -t ashualp:pycodev1 -f  alpine.dockerfile  . 

```

### creating container and checking logs 

```
220  docker run -itd --name ashutest1  ashualp:pycodev1 
  221  docker logs  ashutest1
```

### tips 

```
ashu@docker-client ~]$ docker  image  prune 
WARNING! This will remove all dangling images.
Are you sure you want to continue? [y/N] y
Deleted Images:
deleted: sha256:d35131a5ee79243d462185d9769ed2bee8bc10e7173e05714f2451bf90c88d19
deleted: sha256:a96db9163c19529b92443dbfa18b938c2a9db9d4dd1dc81a524a618e1986253f
deleted: sha256:a95e
```

### linux based distro and their software installer

<img src="inst.png">

### Default process


```
from alpine 
LABEL email=ashutoshh@linux.com 
RUN apk add python3 
RUN mkdir  /code 
ADD https://raw.githubusercontent.com/redashu/pythonLang/main/while.py /code/ 
COPY . /code/
# copy and add both are same but add support URL sources also 
WORKDIR /code 
# change working directory to this 
ENTrypoint ["python3"]
CMD ["while.py"]

# ENTrypoint is same as CMD 
```

### dockeringore file 

```
Dockerfile
*.dockerfile
.dockerignore

```

### to create containers 

```
docker  run -itd --name ashuc3 ashualp:v3
docker  run -itd --name ashuc4 ashualp:v3 mobi.py 

```

## java sample code 

### example 1. 

```
class myclass { 
    public static void main(String args[]) 
    { 
        // test expression 
        while (true) { 
            System.out.println("Hello World this is ashu "); 
  
            // update expression 
        } 
    } 
} 
```

### Dockerfile 

```
FROM openjdk
LABEL name=ashutoshh
RUN mkdir /javacode 
ADD ashu.java /javacode/
WORKDIR  /javacode
# changing directory during image build time
RUN javac ashu.java 
# compiling code 
CMD ["java","myclass"]
# to run we need class name 
# to fix default process for this docker image


```

### lets build it  and check 

```
[ashu@docker-client java_images]$ ls
ashu.java  Dockerfile
[ashu@docker-client java_images]$ docker build -t  ashujava:codev1 . 
Sending build context to Docker daemon  3.072kB
Step 1/7 : FROM openjdk
 ---> b83a192caadf
Step 2/7 : LABEL name=ashutoshh
 ---> Running in a8b6f79b9a92
Removing intermediate container a8b6f79b9a92
 ---> 12e429b985dd
Step 3/7 : RUN mkdir /javacode
 ---> Running in dd5be33c52bd
Removing intermediate container dd5be33c52bd
 ---> 1d5b1de5260c
Step 4/7 : ADD ashu.java /javacode/
 ---> 666cc7a8270b
Step 5/7 : WORKDIR  /javacode
 ---> Running in 5814d65f8dab
Removing intermediate container 5814d65f8dab
 ---> c77858cbff16
Step 6/7 : RUN javac ashu.java
 ---> Running in bac6601899bf
Removing intermediate container bac6601899bf
 ---> 90c2a826a762
Step 7/7 : CMD ["java","myclass"]
 ---> Running in 5f0601366515
Removing intermediate container 5f0601366515
 ---> 080b91be95cc
Successfully built 080b91be95cc
Successfully tagged ashujava:codev1
```

### testing 

```
 docker images   |   grep ashu 
ashujava          codev1       080b91be95cc   About a minute ago       464MB
ashualp           v3           fbe84c010e76   53 minutes ago           55.8MB
ashualp           cmd          db648b59f2e2   About an hour ago        55.8MB
ashualp           entrypoint   8faa606992bf   About an hour ago        55.8MB
ashualp           pycodev1     8faa606992bf   About an hour ago        55.8MB
ashupython        v2           0787903ace71   2 hours ago              142MB
ashupython        v1           7014c4eb0648   2 hours ago              920MB
[ashu@docker-client ~]$ docker  run -itd --name ashujc1  080b91be95cc 
418843ddbe80bc1d66fe5db529ea1f1bffafcd6f38babb8cdbc2edc2e06232e6
[ashu@docker-client ~]$ docker  ps
CONTAINER ID   IMAGE             COMMAND           CREATED              STATUS              PORTS     NAMES
418843ddbe80   080b91be95cc      "java myclass"    4 seconds ago        Up 2 seconds                  ashujc1
783a930bbbe6   yasminjava:v1     "java myclass"    32 seconds ago       Up 26 seconds                 c1
de3b8d004884   sarkany-java:v1   "java Sample"     About a minute ago   Up About a minute             sarkanyc1
6a23f497897f   barisjava:v1      "java baris"      3 minutes ago        Up 3 minutes                  barisjavac
351361e13f6e   abhijava:v1       "java abhishek"   4 minutes ago        Up 4 minutes                  abhijavac1
8b4b2a4fa3ab   peterjava:v1      "java peter"      4 minutes ago        Up 4 minutes                  peterj1
[ashu@docker-client ~]$ docker  logs  ashujc1 
Hello World this is ashu 
Hello World this is ashu 
Hello World this is ashu 
Hello World this is ashu 
Hello World this is ashu 

```

