# nginx

controlplane $ git init
Initialized empty Git repository in /root/.git/
controlplane $ 
controlplane $ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
Username for 'https://github.com': Aarthi-13
Password for 'https://Aarthi-13@github.com': 
remote: Enumerating objects: 18, done.
remote: Counting objects: 100% (18/18), done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 18 (delta 2), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (18/18), done.
controlplane $ ls
go  nginx
controlplane $ cd nginx
controlplane $ ls
nginx-po.yaml  nginx-svc.yaml  README.md
controlplane $ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
controlplane $ kubectl create -f nginx-po.yaml
deployment.apps/nginx-deployment created
controlplane $ kubectl create -f nginx-svc.yamk
error: the path "nginx-svc.yamk" does not exist
controlplane $ kubectl create -f nginx-svc.yaml
service/my-service created
controlplane $ kubectl get po
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-69f46cd759-hctlh   1/1     Running   0          78s
controlplane $ kubectl get po -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
nginx-deployment-69f46cd759-hctlh   1/1     Running   0          87s   10.244.1.3   node01   <none>           <none>
controlplane $ kubectl describe svc my-service
Name:                     my-service
Namespace:                default
Labels:                   app=nginx-app
Annotations:              <none>
Selector:                 app=nginx-app
Type:                     NodePort
IP:                       10.103.194.24
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31111/TCP
Endpoints:                10.244.1.3:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
controlplane $ 
controlplane $ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   1/1     1            1           119s
controlplane $ 
controlplane $ kubectl get services
NAME         TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP        33m
my-service   NodePort    10.103.194.24   <none>        80:31111/TCP   105s
controlplane $ 
controlplane $ kubectl get deploy --all-namespaces
NAMESPACE     NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
default       nginx-deployment          1/1     1            1           2m25s
kube-system   coredns                   2/2     2            2           33m
kube-system   katacoda-cloud-provider   0/1     1            0           33m
controlplane $ 
controlplane $ kubectl delete -f nginx-po.yaml
deployment.apps "nginx-deployment" deleted
controlplane $ kubectl delete -f nginx-svc.yaml
service "my-service" deleted
controlplane $ kubectl get deploy
No resources found in default namespace.
controlplane $ kubectl get pod
No resources found in default namespace.
controlplane $ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   34m
controlplane $ 
