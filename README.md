## training plan 

<img src="plan.png">

### deploy app in prod env -- and problem with external LB 

<img src="lb.png">

### ingress products 

<img src="ingress.png">

### Deploy Ingress 

```
 kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.2.0/deploy/static/provider/baremetal/deploy.yaml
namespace/ingress-nginx created
serviceaccount/ingress-nginx created
serviceaccount/ingress-nginx-admission created
role.rbac.authorization.k8s.io/ingress-nginx created
role.rbac.authorization.k8s.io/ingress-nginx-admission created
clusterrole.rbac.authorization.k8s.io/ingress-nginx unchanged
clusterrole.rbac.authorization.k8s.io/ingress-nginx-admission unchanged
rolebinding.rbac.authorization.k8s.io/ingress-nginx created
rolebinding.rbac.authorization.k8s.io/ingress-nginx-admission
```

### lets verify 

```
kubectl get  deploy -n ingress-nginx
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
ingress-nginx-controller   1/1     1            1           50s
[ashu@docker-client mobi-dockerimages]$ kubectl get  rs  -n ingress-nginx
NAME                                  DESIRED   CURRENT   READY   AGE
ingress-nginx-controller-6b864cf6dd   1         1         1       64s
[ashu@docker-client mobi-dockerimages]$ kubectl get  po  -n ingress-nginx
NAME                                        READY   STATUS      RESTARTS   AGE
ingress-nginx-admission-create-r6wm8        0/1     Completed   0          72s
ingress-nginx-admission-patch-gbnhr         0/1     Completed   1          72s
ingress-nginx-controller-6b864cf6dd-6d4bw   1/1     Running     0          72s
[ashu@docker-client mobi-dockerimages]$ kubectl get  svc  -n ingress-nginx
NAME                                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             NodePort    10.106.192.32    <none>        80:31034/TCP,443:32642/TCP   95s
ingress-nginx-controller-admission   ClusterIP   10.108.180.148   <none>        443/TCP                      95s
[ashu@docker-client mobi-dockerimages]$ 
```

### changes in container 

```
docker  exec -it  ashuc1 bash 
root@c4b20f5bf5f4:/# 
root@c4b20f5bf5f4:/# 
root@c4b20f5bf5f4:/# cd /usr/share/nginx/html/
root@c4b20f5bf5f4:/usr/share/nginx/html# apt udpate ; apt install vim -y ^C
root@c4b20f5bf5f4:/usr/share/nginx/html# ls
50x.html  dog.jpg  index.html
root@c4b20f5bf5f4:/usr/share/nginx/html# cat  index.html 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ashu</title>
</head>
<body>
    <h1> i am ashu and i love dogs  </h1>
    <img src="dog.jpg" alt="not loading ">
    
</body>
</html>
```

### changing a running container into docker image 

```
docker  commit  ashuc1  dockerashu/ashuapp:dogv1 
sha256:461d08a6358cf20911e82f476a37efa97674ad508042bfd6022dbc156e11866d
[ashu@docker-client mobi-dockerimages]$ 
[ashu@docker-client mobi-dockerimages]$ docker login -u dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@docker-client mobi-dockerimages]$ docker push dockerashu/ashuapp:dogv1
The push refers to repository [docker.io/dockerashu/ashuapp]
0d570e5bfe27: Pushed 
31905c988dca: Mounted from dockerashu/dog 
431a31bd0b21: Mounted from dockerashu/dog 
```

### application deploy with nginx 

### create deployment 

```
 mkdir ingress-demo 
[ashu@docker-client k8s-deploy-apps]$ cd ingress-demo/
[ashu@docker-client ingress-demo]$ 
[ashu@docker-client ingress-demo]$ kubectl create deployment ashuapp --image=dockerashu/ashuapp:dogv1    --port 80  
deployment.apps/ashuapp created
[ashu@docker-client ingress-demo]$ kubectl get deploy 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashuapp   1/1     1            1           20s
[ashu@docker-client ingress-demo]$ 

```

### creating service 

```
kubectl get deploy 
NAME      READY   UP-TO-DATE   AVAILABLE   AGE
ashuapp   1/1     1            1           2m50s
[ashu@docker-client ingress-demo]$ kubectl expose deploy ashuapp --type ClusterIP --port 80 --name  ashulb1 
service/ashulb1 exposed
[ashu@docker-client ingress-demo]$ kubectl get svc
NAME      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
ashulb1   ClusterIP   10.102.175.126   <none>        80/TCP    4s
```

### Ingress rule 

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ashu-ingress-rule
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx
  rules:
  - host: www.ashumobi.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: ashulb1
            port:
              number: 80
```

### deploy it 

```
kubectl apply -f rule.yaml 
ingress.networking.k8s.io/ashu-ingress-rule created
[ashu@docker-client ingress-demo]$ kubectl get ingress 
NAME                CLASS   HOSTS              ADDRESS   PORTS   AGE
ashu-ingress-rule   nginx   www.ashumobi.com             80      11s
[ashu@docker-client ingress-demo]$ kubectl get ingress 
NAME                CLASS   HOSTS              ADDRESS         PORTS   AGE
ashu-ingress-rule   nginx   www.ashumobi.com   172.31.11.164   80      38s
[ashu@docker-client ingress-demo]$ 

```

### few problem in CD process if we consider jenkins 

<img src="jprob.png">

### deploy argocd in k8s 

```
kubectl create namespace argocd
namespace/argocd created
[ashu@docker-client ingress-demo]$ kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
customresourcedefinition.apiextensions.k8s.io/applications.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/applicationsets.argoproj.io created
customresourcedefinition.apiextensions.k8s.io/appprojects.argoproj.io created
serviceaccount/argocd-application-controller created
serviceaccount/argocd-applicationset-controller created
serviceaccount/argocd-dex-server created
serviceaccount/argocd-notifications-controller cr
```

### change service to Nodeport if you dont' have ingress rules 

```
kubectl get deploy -n argocd 
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
argocd-applicationset-controller   1/1     1            1           73s
argocd-dex-server                  1/1     1            1           73s
argocd-notifications-controller    1/1     1            1           73s
argocd-redis                       1/1     1            1           73s
argocd-repo-server                 1/1     1            1           72s
argocd-server                      1/1     1            1           72s
[ashu@docker-client ingress-demo]$ kubectl get svc  -n argocd 
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.101.2.253     <none>        7000/TCP,8080/TCP            3m26s
argocd-dex-server                         ClusterIP   10.105.104.15    <none>        5556/TCP,5557/TCP,5558/TCP   3m26s
argocd-metrics                            ClusterIP   10.100.164.156   <none>        8082/TCP                     3m26s
argocd-notifications-controller-metrics   ClusterIP   10.100.63.251    <none>        9001/TCP                     3m26s
argocd-redis                              ClusterIP   10.102.143.131   <none>        6379/TCP                     3m26s
argocd-repo-server                        ClusterIP   10.106.88.201    <none>        8081/TCP,8084/TCP            3m26s
argocd-server                             ClusterIP   10.108.8.126     <none>        80/TCP,443/TCP               3m26s
argocd-server-metrics                     ClusterIP   10.99.161.50     <none>        8083/TCP                     3m26s
[ashu@docker-client ingress-demo]$ kubectl edit svc argocd-server  -n argocd 
service/argocd-server edited
[ashu@docker-client ingress-demo]$ kubectl get svc  -n argocd 
NAME                                      TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
argocd-applicationset-controller          ClusterIP   10.101.2.253     <none>        7000/TCP,8080/TCP            4m27s
argocd-dex-server                         ClusterIP   10.105.104.15    <none>        5556/TCP,5557/TCP,5558/TCP   4m27s
argocd-metrics                            ClusterIP   10.100.164.156   <none>        8082/TCP                     4m27s
argocd-notifications-controller-metrics   ClusterIP   10.100.63.251    <none>        9001/TCP                     4m27s
argocd-redis                              ClusterIP   10.102.143.131   <none>        6379/TCP                     4m27s
argocd-repo-server                        ClusterIP   10.106.88.201    <none>        8081/TCP,8084/TCP            4m27s
argocd-server                             NodePort    10.108.8.126     <none>        80:32523/TCP,443:32598/TCP   4m27s
argocd-server-metrics                     ClusterIP   10.99.161.50     <none>        8083/TCP                     4m27s
[ashu@docker-client ingress-demo]$ 
```

### to get password  for default admin user 

```
 kubectl get secret -n argocd 
NAME                          TYPE     DATA   AGE
argocd-initial-admin-secret   Opaque   1      3m9s
argocd-notifications-secret   Opaque   0      3m16s
argocd-secret                 Opaque   5      3m16s
```

###

```
kubectl get secret  argocd-initial-admin-secret   -n argocd -o yaml 
apiVersion: v1
data:
  password: c25FdGdNR1RFSzk2dHpoag==
kind: Secret
metadata:

```

### 

```
echo c25FdGdNR1RFSzk2dHpoag==  |  base64 -d
snEtgMGTEK96tzhj
```

### HELM In k8s 

<img src="helm.png">

### installing on client side 

```

[root@docker-client ~]# ls
apachespark  helm-v3.9.0-linux-amd64.tar.gz  nginx.tar  users.txt
[root@docker-client ~]# tar xvf helm-v3.9.0-linux-amd64.tar.gz 
linux-amd64/
linux-amd64/helm
linux-amd64/LICENSE
linux-amd64/README.md
[root@docker-client ~]# ls
apachespark  helm-v3.9.0-linux-amd64.tar.gz  linux-amd64  nginx.tar  users.txt
[root@docker-client ~]# cd linux-amd64/
[root@docker-client linux-amd64]# ls
helm  LICENSE  README.md
[root@docker-client linux-amd64]# mv helm  /usr/bin/
[root@docker-client linux-amd64]# helm version 
version.BuildInfo{Version:"v3.9.0", GitCommit:"7ceeda6c585217a19a1131663d8cd1f7d641b2a7", GitTreeState:"clean", GoVersion:"go1.17.5"}
[root@docker-client linux-amd64]# 

```


