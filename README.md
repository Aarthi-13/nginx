# nginx

controlplane $ git init
Initialized empty Git repository in /root/.git/
controlplane $ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
Username for 'https://github.com': Aarthi-13
Password for 'https://Aarthi-13@github.com': 
remote: Enumerating objects: 24, done.
remote: Counting objects: 100% (24/24), done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 24 (delta 4), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (24/24), done.
controlplane $ ls
go  nginx
controlplane $ cd nginx
controlplane $ ls
nginx-po.yaml  nginx-svc.yaml  README.md
controlplane $ 
controlplane $ kubectl create -f nginx-po.yaml
deployment.apps/nginx-deployment created
controlplane $ kubectl create -f nginx-svc.yaml
service/my-service created
controlplane $ kubectl get po
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-69f46cd759-4v7k4   0/1     ContainerCreating   0          20s
nginx-deployment-69f46cd759-htdxm   0/1     ContainerCreating   0          20s
nginx-deployment-69f46cd759-zwmcs   0/1     ContainerCreating   0          20s
controlplane $ kubectl get po
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-69f46cd759-4v7k4   1/1     Running   0          54s
nginx-deployment-69f46cd759-htdxm   1/1     Running   0          54s
nginx-deployment-69f46cd759-zwmcs   1/1     Running   0          54s
controlplane $ kubectl get po -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
nginx-deployment-69f46cd759-4v7k4   1/1     Running   0          62s   10.244.1.3   node01   <none>           <none>
nginx-deployment-69f46cd759-htdxm   1/1     Running   0          62s   10.244.1.4   node01   <none>           <none>
nginx-deployment-69f46cd759-zwmcs   1/1     Running   0          62s   10.244.1.5   node01   <none>           <none>
controlplane $ kubectl get services
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP        2m15s
my-service   NodePort    10.105.104.253   <none>        80:31111/TCP   64s
controlplane $ kubectl getdeploy
Error: unknown command "getdeploy" for "kubectl"
Run 'kubectl --help' for usage.
controlplane $ kubectl get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
nginx-deployment   3/3     3            3           90s
controlplane $ kubectl get deploy --all-namespaces
NAMESPACE     NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
default       nginx-deployment          3/3     3            3           104s
kube-system   coredns                   2/2     2            2           2m42s
kube-system   katacoda-cloud-provider   1/1     1            1           2m40s
controlplane $ kubectl describe svc my-service
Name:                     my-service
Namespace:                default
Labels:                   app=nginx-app
Annotations:              <none>
Selector:                 app=nginx-app
Type:                     NodePort
IP:                       10.105.104.253
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  31111/TCP
Endpoints:                10.244.1.3:80,10.244.1.4:80,10.244.1.5:80
Session Affinity:         None
External Traffic Policy:  Cluster
Events:                   <none>
controlplane $ curl http://10.244.1.3:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
controlplane $ curl http://10.244.1.4:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
controlplane $ curl http://10.244.1.5:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
controlplane $ kubectl get service my-service
NAME         TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
my-service   NodePort   10.105.104.253   <none>        80:31111/TCP   2m51s
controlplane $ curl http://10.105.104.253:80
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
controlplane $ 
controlplane $ kubectl delete -f nginx-po.yaml
deployment.apps "nginx-deployment" deleted
controlplane $ kubectl get deploy
No resources found in default namespace.
controlplane $ kubectl delete -f nginx-svc.yaml
service "my-service" deleted
controlplane $ kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5m37s
controlplane $ kubectl get po
No resources found in default namespace.
controlplane $ 
