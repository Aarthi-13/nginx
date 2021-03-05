```bash

[node1 ~]$ kubectl get nodes
NAME    STATUS   ROLES                  AGE   VERSION
node1   Ready    control-plane,master   30m   v1.20.1
node2   Ready    <none>                 59s   v1.20.1
node3   Ready    <none>                 53s   v1.20.1
node4   Ready    <none>                 27s   v1.20.1
[node1 ~]$
[node1 ~]$ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
Username for 'https://github.com': Aarthi-13
Password for 'https://Aarthi-13@github.com':
remote: Enumerating objects: 66, done.
remote: Counting objects: 100% (66/66), done.
remote: Compressing objects: 100% (63/63), done.
remote: Total 66 (delta 24), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (66/66), done.
[node1 ~]$ ls
anaconda-ks.cfg  nginx
[node1 ~]$ cd nginx
[node1 nginx]$ ls
README.md  nginx-cm.yaml  nginx-po.yaml  nginx-pv.yaml  nginx-pvc.yaml  nginx-svc.yaml
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
[node1 nginx]$ kubectl get po
NAME               READY   STATUS              RESTARTS   AGE
fluentd-ds-chnt8   0/1     ContainerCreating   0          9s
fluentd-ds-gm85k   0/1     ContainerCreating   0          9s
fluentd-ds-zfllz   0/1     ContainerCreating   0          9s
[node1 nginx]$ kubectl get po
NAME               READY   STATUS    RESTARTS   AGE
fluentd-ds-chnt8   1/1     Running   0          2m4s
fluentd-ds-gm85k   1/1     Running   0          2m4s
fluentd-ds-zfllz   1/1     Running   0          2m4s
[node1 nginx]$
[node1 nginx]$
[node1 nginx]$ kubectl describe ds
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
Events:
  Type    Reason            Age    From                  Message
  ----    ------            ----   ----                  -------
  Normal  SuccessfulCreate  2m48s  daemonset-controller  Created pod: fluentd-ds-chnt8
  Normal  SuccessfulCreate  2m48s  daemonset-controller  Created pod: fluentd-ds-gm85k
  Normal  SuccessfulCreate  2m48s  daemonset-controller  Created pod: fluentd-ds-zfllz
[node1 nginx]$ kubectl get po -o wide
NAME               READY   STATUS    RESTARTS   AGE     IP         NODE    NOMINATED NODE   READINESS GATES
fluentd-ds-chnt8   1/1     Running   0          3m37s   10.5.3.2   node4   <none>           <none>
fluentd-ds-gm85k   1/1     Running   0          3m37s   10.5.1.2   node2   <none>           <none>
fluentd-ds-zfllz   1/1     Running   0          3m37s   10.5.2.2   node3   <none>           <none>
[node1 nginx]$ kubectl get nodes
NAME    STATUS   ROLES                  AGE     VERSION
node1   Ready    control-plane,master   36m     v1.20.1
node2   Ready    <none>                 7m17s   v1.20.1
node3   Ready    <none>                 7m11s   v1.20.1
node4   Ready    <none>                 6m45s   v1.20.1
node5   Ready    <none>                 32s     v1.20.1
[node1 nginx]$ kubectl get po -o wide
NAME               READY   STATUS              RESTARTS   AGE     IP         NODE    NOMINATED NODE   READINESS GATES
fluentd-ds-chnt8   1/1     Running             0          4m19s   10.5.3.2   node4   <none>           <none>
fluentd-ds-gm85k   1/1     Running             0          4m19s   10.5.1.2   node2   <none>           <none>
fluentd-ds-jgcvl   0/1     ContainerCreating   0          31s     <none>     node5   <none>           <none>
fluentd-ds-zfllz   1/1     Running             0          4m19s   10.5.2.2   node3   <none>           <none>
[node1 nginx]$ kubectl get po -o wide
NAME               READY   STATUS    RESTARTS   AGE     IP         NODE    NOMINATED NODE   READINESS GATES
fluentd-ds-chnt8   1/1     Running   0          4m39s   10.5.3.2   node4   <none>           <none>
fluentd-ds-gm85k   1/1     Running   0          4m39s   10.5.1.2   node2   <none>           <none>
fluentd-ds-jgcvl   1/1     Running   0          51s     10.5.4.2   node5   <none>           <none>
fluentd-ds-zfllz   1/1     Running   0          4m39s   10.5.2.2   node3   <none>           <none>
[node1 nginx]$ kubectl delete ds fluentd-ds
daemonset.apps "fluentd-ds" deleted
[node1 nginx]$ kubectl get po
NAME               READY   STATUS        RESTARTS   AGE
fluentd-ds-chnt8   1/1     Terminating   0          5m30s
fluentd-ds-jgcvl   1/1     Terminating   0          102s
[node1 nginx]$ kubectl get po
No resources found in default namespace.
[node1 nginx]$
[node1 nginx]$
