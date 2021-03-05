```bash
pip install foobar
```
controlplane $ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
Username for 'https://github.com': Aarthi-13
Password for 'https://Aarthi-13@github.com': 
remote: Enumerating objects: 36, done.
remote: Counting objects: 100% (36/36), done.
remote: Compressing objects: 100% (33/33), done.
remote: Total 36 (delta 8), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (36/36), done.
controlplane $ ls
go  nginx
controlplane $ cd nginx
controlplane $ ls
nginx-cm.yaml  nginx-po.yaml  nginx-svc.yaml  README.md
controlplane $ git checkout DaemonSet
Branch 'DaemonSet' set up to track remote branch 'DaemonSet' from 'origin'.
Switched to a new branch 'DaemonSet'
controlplane $ git branch
* DaemonSet
  main
controlplane $ ls
nginx-cm.yaml  nginx-ds.yaml  nginx-po.yaml  nginx-svc.yaml  README.md
controlplane $ kubectl create -f nginx-ds.yaml
daemonset.apps/fluentd-ds created
controlplane $ kubectl get po -o wide
NAME               READY   STATUS              RESTARTS   AGE   IP       NODE     NOMINATED NODE   READINESS GATES
fluentd-ds-fvg6w   0/1     ContainerCreating   0          16s   <none>   node01   <none>           <none>
controlplane $ kubectl get po -o wide
NAME               READY   STATUS    RESTARTS   AGE   IP           NODE     NOMINATED NODE   READINESS GATES
fluentd-ds-fvg6w   1/1     Running   0          80s   10.244.1.3   node01   <none>           <none>
controlplane $ kubectl get ds
NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
fluentd-ds   1         1         1       1            1           <none>          91s
controlplane $ kubectl describe ds
Name:           fluentd-ds
Selector:       name=fluentd
Node-Selector:  <none>
Labels:         <none>
Annotations:    deprecated.daemonset.template.generation: 1
Desired Number of Nodes Scheduled: 1
Current Number of Nodes Scheduled: 1
Number of Nodes Scheduled with Up-to-date Pods: 1
Number of Nodes Scheduled with Available Pods: 1
Number of Nodes Misscheduled: 0
Pods Status:  1 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  name=fluentd
  Containers:
   fluentd:
    Image:        gcr.io/google-containers/fluentd-elasticsearch:1.20
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:
  Type    Reason            Age   From                  Message
  ----    ------            ----  ----                  -------
  Normal  SuccessfulCreate  104s  daemonset-controller  Created pod: fluentd-ds-fvg6w
controlplane $ 
