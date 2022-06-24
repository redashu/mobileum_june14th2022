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
