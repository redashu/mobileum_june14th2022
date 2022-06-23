## training plan 

<img src="plan.png">

## Revision 

<img src="rev.png">

### Installation methods of k8s on various platforms 

<img src="installk8s.png">

### Minikube to deploy k8s setup 

<img src="minikube.png">

### installing minikube 

```
 
[root@ip-172-31-91-143 ~]# curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 71.4M  100 71.4M    0     0   223M      0 --:--:-- --:--:-- --:--:--  224M
[root@ip-172-31-91-143 ~]# sudo install minikube-linux-amd64 /usr/local/bin/minikube
[root@ip-172-31-91-143 ~]# 
[root@ip-172-31-91-143 ~]# logout
[ec2-user@ip-172-31-91-143 ~]$ sudo -i
[root@ip-172-31-91-143 ~]# cp /usr/local/bin/minikube /usr/bin/
[root@ip-172-31-91-143 ~]# 
[root@ip-172-31-91-143 ~]# minikube version 
minikube version: v1.26.0
commit: f4b412861bb746be73053c9f6d2895f12cf78565
```

### add non root user to docker group 

```
usermod -aG docker  ec2-user

```

### deploy single node k8s cluster using minikube -- master / minion will be on the same node 

```
minikube start  --driver=docker 
😄  minikube v1.26.0 on Amazon 2
✨  Using the docker driver based on user configuration
📌  Using Docker driver with root privileges
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.24.1 preload ...
    > preloaded-images-k8s-v18-v1...: 405.83 MiB / 405.83 M
```

### setup client 

```
 curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   154  100   154    0     0   3745      0 --:--:-- --:--:-- --:--:--  3756
100 43.5M  100 43.5M    0     0   167M      0 --:--:-- --:--:-- --:--:--  167M
[ec2-user@ip-172-31-91-143 ~]$ ls
kubectl
[ec2-user@ip-172-31-91-143 ~]$ sudo mv kubectl /usr/bin/
[ec2-user@ip-172-31-91-143 ~]$ sudo chmod +x /usr/bin/kubectl 
[ec2-user@ip-172-31-91-143 ~]$ 
[ec2-user@ip-172-31-91-143 ~]$ kubectl get nodes
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   64s   v1.24.1
[ec2-user@ip-172-31-91-143 ~]$ 


```




