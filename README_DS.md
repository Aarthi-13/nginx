```bash

[node1 ~]$ kubectl get nodes
NAME    STATUS   ROLES                  AGE     VERSION
node1   Ready    control-plane,master   9m18s   v1.20.1
node2   Ready    <none>                 43s     v1.20.1
node4   Ready    <none>                 37s     v1.20.1
node5   Ready    <none>                 33s     v1.20.1
[node1 ~]$
[node1 ~]$
[node1 ~]$ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
Username for 'https://github.com': Aarthi-13
Password for 'https://Aarthi-13@github.com':
remote: Enumerating objects: 45, done.
remote: Counting objects: 100% (45/45), done.
remote: Compressing objects: 100% (42/42), done.
remote: Total 45 (delta 13), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (45/45), done.
[node1 ~]$ ls
anaconda-ks.cfg  nginx
[node1 ~]$ cd nginx
[node1 nginx]$ ls
README.md  nginx-cm.yaml  nginx-po.yaml  nginx-svc.yaml
[node1 nginx]$ git branch
* main
[node1 nginx]$ git checkout DaemonSet
Branch DaemonSet set up to track remote branch DaemonSet from origin.
Switched to a new branch 'DaemonSet'
[node1 nginx]$ git branch
* DaemonSet
  main
[node1 nginx]$ ls
README.md  README_DS.md  nginx-cm.yaml  nginx-ds.yaml  nginx-po.yaml  nginx-svc.yaml
[node1 nginx]$ kubectl create -f nginx-ds.yaml
daemonset.apps/fluentd-ds created
[node1 nginx]$ kubectl get pods
NAME               READY   STATUS    RESTARTS   AGE
fluentd-ds-j84df   1/1     Running   0          31s
fluentd-ds-vv5bf   1/1     Running   0          31s
fluentd-ds-w754h   1/1     Running   0          31s
[node1 nginx]$ kubectl get pods -o wide
NAME               READY   STATUS    RESTARTS   AGE   IP         NODE    NOMINATED NODE   READINESS GATES
fluentd-ds-j84df   1/1     Running   0          36s   10.5.1.2   node2   <none>           <none>
fluentd-ds-vv5bf   1/1     Running   0          36s   10.5.2.2   node4   <none>           <none>
fluentd-ds-w754h   1/1     Running   0          36s   10.5.3.2   node5   <none>           <none>
[node1 nginx]$ kubectl get ds
NAME         DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
fluentd-ds   3         3         3       3            3           <none>          53s
[node1 nginx]$ kubectl describe ds fluentd-ds
Name:           fluentd-ds
Selector:       name=fluentd
Node-Selector:  <none>
Labels:         <none>
Annotations:    deprecated.daemonset.template.generation: 1
Desired Number of Nodes Scheduled: 3
Current Number of Nodes Scheduled: 3
Number of Nodes Scheduled with Up-to-date Pods: 3
Number of Nodes Scheduled with Available Pods: 3
Number of Nodes Misscheduled: 0
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
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
  ber of Nodes Scheduled: 3
Number of Nodes Scheduled with Up-to-date Pods: 3
Number of Nodes Scheduled with Available Pods: 3
Number of Nodes Misscheduled: 0
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
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
  Normal  SuccessfulCreate  75s   daemonset-controller  Created pod: fluentd-ds-j84df
  Normal  SuccessfulCreate  75s   daemonset-controller  Created pod: fluentd-ds-w754h
  Normal  SuccessfulCreate  75s   daemonset-controller  Created pod: fluentd-ds-vv5bf
[node1 nginx]$
