## training plan 

<img src="plan.png">

### getting started with Namespace and further deployments 

```
 kubectl config get-contexts 
CURRENT   NAME                          CLUSTER      AUTHINFO           NAMESPACE
*         kubernetes-admin@kubernetes   kubernetes   kubernetes-admin   ashu-project
[ashu@docker-client k8s-deploy-apps]$ kubectl get po 
No resources found in ashu-project namespace.
[ashu@docker-client k8s-deploy-apps]$ kubectl get svc
No resources found in ashu-project namespace.
[ashu@docker-client k8s-deploy-apps]$ 

```

### redeploy pod in custom namespace 

```
kubectl create -f  customerapp.yaml 
pod/ashucustomer created
[ashu@docker-client k8s-deploy-apps]$ kubectl get pods
NAME           READY   STATUS    RESTARTS   AGE
ashucustomer   1/1     Running   0          5s
[ashu@docker-client k8s-deploy-apps]$ kubectl get pods -owide
NAME           READY   STATUS    RESTARTS   AGE   IP               NODE      NOMINATED NODE   READINESS GATES
ashucustomer   1/1     Running   0          8s    192.168.50.212   minion3   <none>           <none>
[ashu@docker-client k8s-deploy-apps]$ kubectl get pods --show-labels 
NAME           READY   STATUS    RESTARTS   AGE   LABELS
ashucustomer   1/1     Running   0          17s   x1=helloashu
```

### creating service using two diff methods 

```
 964  kubectl get po --show-labels 
  965  kubectl  create  service nodeport  ashulb2 --tcp 1234:80 --dry-run=client -oyaml 
  966  kubectl  expose  pod  ashucustomer  --type NodePort --port 1234 --target-port 80 --name ashulb2 --dry-run=client -o yaml 

```

### creating service 

```
      6s    192.168.34.19   minion1   <none>           <none>
[ashu@docker-client k8s-deploy-apps]$ kubectl  get po --show-labels 
NAME           READY   STATUS    RESTARTS   AGE     LABELS
ashucustomer   1/1     Running   0          2m34s   x1=helloashu
[ashu@docker-client k8s-deploy-apps]$ kubectl  expose pod ashucustomer  --type NodePort --port 1234 --target-port 80 --name ashulb2  --dry-run=client -o yaml >nodeport2.yaml 
[ashu@docker-client k8s-deploy-apps]$ kubectl create -f nodeport2.yaml 
service/ashulb2 created
[ashu@docker-client k8s-deploy-apps]$ kubectl get svc 
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
ashulb2   NodePort   10.106.214.42   <none>        1234:31141/TCP   4s
[ashu@docker-client k8s-deploy-apps]$ kubectl get svc -owide
NAME      TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE   SELECTOR
ashulb2   NodePort   10.106.214.42   <none>        1234:31141/TCP   7s    x1=helloashu
[ashu@docker-client k8s-deploy-apps]$ 
```
