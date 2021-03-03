# nginx

controlplane $ vi nginx-po.yaml
controlplane $ vi nginx-svc.yaml
controlplane $ kubectl create -f nginx-po.yaml
deployment.apps/nginx-deployment created
controlplane $ kubectl create -f nginx-svc.yaml
service/my-service created
controlplane $ 
controlplane $ kubectl get po
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-69f46cd759-vxpxm   1/1     Running   0          112s
controlplane $ 
controlplane $ kubectl get po -o wide
NAME                                READY   STATUS    RESTARTS   AGE     IP           NODE     NOMINATED NODE   READINESS GATES
nginx-deployment-69f46cd759-vxpxm   1/1     Running   0          2m24s   10.244.1.3   node01   <none>           <none>
controlplane $
controlplane $ kubectl describe svc my-service
Name:                     my-service
Namespace:                default
Labels:                   app=nginx-app
Annotations:              <none>
Selector:                 app=nginx-app
Type:                     NodePort
IP:                       10.109.76.78
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
nginx-deployment   1/1     1            1           9m19s
controlplane $ 
controlplane $ kubectl get services
NAME         TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
kubernetes   ClusterIP   10.96.0.1      <none>        443/TCP        12m
my-service   NodePort    10.109.76.78   <none>        80:31111/TCP   3m47s
controlplane $ 
controlplane $ kubectl get deploy --all-namespaces
NAMESPACE     NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
default       nginx-deployment          1/1     1            1           10m
kube-system   coredns                   2/2     2            2           19m
kube-system   katacoda-cloud-provider   0/1     1            0           19m
controlplane $ 
controlplane $ kubectl delete -f nginx-po.yaml
deployment.apps "nginx-deployment" deleted
controlplane $ kubectl delete -f nginx-svc.yaml
service "my-service" deleted
controlplane $ kubectl get deploy
No resources found in default namespace.
controlplane $ 
controlplane $ kubectl get service
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   23m
controlplane $ kubectl get po
No resources found in default namespace.
controlplane $ 
