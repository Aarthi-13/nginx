```bash
arames625@INSML-3YMD6M git % ls
nginx
arames625@INSML-3YMD6M git % cd nginx
arames625@INSML-3YMD6M nginx % ls
Prometheus	README.md	logging		nginx-cm.yaml	nginx-po.yaml	nginx-svc.yaml	prometheus-sts
arames625@INSML-3YMD6M nginx % cd prometheus-sts
arames625@INSML-3YMD6M prometheus-sts % ls
graf-cm.yaml	graf-sc.yaml	graf-sv.yaml	prom-cm.yaml	prom-pvc.yaml	prom-sts.yaml	sts.yaml
graf-sa.yaml	graf-sts.yaml	ns.yaml		prom-pv.yaml	prom-sa.yaml	prom-sv.yaml
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f ns.yaml
namespace/monitoring created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f prom-cm.yaml
configmap/prometheus-config created
arames625@INSML-3YMD6M prometheus-sts % kubectl apply -f prom-sa.yaml
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRole is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRole
clusterrole.rbac.authorization.k8s.io/prometheus unchanged
serviceaccount/prometheus created
Warning: rbac.authorization.k8s.io/v1beta1 ClusterRoleBinding is deprecated in v1.17+, unavailable in v1.22+; use rbac.authorization.k8s.io/v1 ClusterRoleBinding
clusterrolebinding.rbac.authorization.k8s.io/prometheus unchanged
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f prom-pv.yaml
persistentvolume/pv-volume created
persistentvolume/pv-volume0 created
persistentvolume/pv-volume1 created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f prom-pvc.yaml 
persistentvolumeclaim/my-disk-claim created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f sts.yaml
statefulset.apps/prometheus created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f prom-sv.yaml
service/prometheus created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f graf-cm.yaml
configmap/grafana-ini created
configmap/grafana-datasources created
configmap/grafana-dashboardproviders created
configmap/grafana-dashboards created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f graf-sc.yaml
secret/grafana created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f graf-sa.yaml
serviceaccount/grafana created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f graf-sts.yaml
statefulset.apps/grafana created
arames625@INSML-3YMD6M prometheus-sts % kubectl create -f graf-sv.yaml
service/grafana created
arames625@INSML-3YMD6M prometheus-sts % kubectl get pv,pvc -n monitoring
NAME                          CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                               STORAGECLASS   REASON   AGE
persistentvolume/pv-volume    2Gi        RWO            Retain           Bound    monitoring/my-disk-claim            manual                  2m
persistentvolume/pv-volume0   2Gi        RWO            Retain           Bound    monitoring/data-prometheus-0        manual                  2m
persistentvolume/pv-volume1   2Gi        RWO            Retain           Bound    monitoring/grafana-data-grafana-0   manual                  2m

NAME                                           STATUS   VOLUME       CAPACITY   ACCESS MODES   STORAGECLASS   AGE
persistentvolumeclaim/data-prometheus-0        Bound    pv-volume0   2Gi        RWO            manual         101s
persistentvolumeclaim/grafana-data-grafana-0   Bound    pv-volume1   2Gi        RWO            manual         33s
persistentvolumeclaim/my-disk-claim            Bound    pv-volume    2Gi        RWO            manual         108s
arames625@INSML-3YMD6M prometheus-sts % 
arames625@INSML-3YMD6M prometheus-sts % kubectl get all -n monitoring
NAME               READY   STATUS            RESTARTS   AGE
pod/grafana-0      0/1     PodInitializing   0          19s
pod/prometheus-0   1/1     Running           0          87s

NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/grafana      NodePort   10.99.163.8    <none>        3000:30300/TCP   14s
service/prometheus   NodePort   10.103.149.8   <none>        9090:30900/TCP   64s

NAME                          READY   AGE
statefulset.apps/grafana      0/1     19s
statefulset.apps/prometheus   1/1     87s
arames625@INSML-3YMD6M prometheus-sts % kubectl get all -n monitoring   
NAME               READY   STATUS    RESTARTS   AGE
pod/grafana-0      0/1     Running   0          37s
pod/prometheus-0   1/1     Running   0          105s

NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/grafana      NodePort   10.99.163.8    <none>        3000:30300/TCP   32s
service/prometheus   NodePort   10.103.149.8   <none>        9090:30900/TCP   82s

NAME                          READY   AGE
statefulset.apps/grafana      0/1     37s
statefulset.apps/prometheus   1/1     105s
arames625@INSML-3YMD6M prometheus-sts % kubectl describe po grafana-0 -n monitoring
Name:         grafana-0
Namespace:    monitoring
Priority:     0
Node:         minikube/192.168.64.3
Start Time:   Thu, 18 Mar 2021 08:31:44 +0530
Labels:       controller-revision-hash=grafana-7548658bdb
              k8s-app=grafana
              statefulset.kubernetes.io/pod-name=grafana-0
Annotations:  <none>
Status:       Running
IP:           172.17.0.4
IPs:
  IP:           172.17.0.4
Controlled By:  StatefulSet/grafana
Init Containers:
  init-chmod-data:
    Container ID:  docker://a7315b97b4263818b06dad6cf91e8444143279ae6e10a78294846ed8e4268901
    Image:         debian:9
    Image ID:      docker-pullable://debian@sha256:ca2af3c25b43f185cc86cc2038a217d9b4cdbb4d47adbcfe5d25e04c1d75e1d9
    Port:          <none>
    Host Port:     <none>
    Command:
      chmod
      777
      /var/lib/grafana
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Thu, 18 Mar 2021 08:31:52 +0530
      Finished:     Thu, 18 Mar 2021 08:31:52 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/lib/grafana from grafana-data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from grafana-token-6d8cs (ro)
Containers:
  grafana:
    Container ID:   docker://55b0a968c2cbae3c8707be91c6bcc8e728a1d9dfc4886f6ee5a0eba5d0885cae
    Image:          grafana/grafana:6.0.1
    Image ID:       docker-pullable://grafana/grafana@sha256:dcfccebd894c3596827ef7ef5e0a69a669a3a761a19e382f896f11531f45b8fb
    Ports:          80/TCP, 3000/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Thu, 18 Mar 2021 08:32:21 +0530
    Ready:          False
    Restart Count:  0
    Limits:
      cpu:     50m
      memory:  100Mi
    Requests:
      cpu:      50m
      memory:   100Mi
    Liveness:   http-get http://:3000/api/health delay=0s timeout=1s period=10s #success=1 #failure=3
    Readiness:  http-get http://:3000/api/health delay=60s timeout=30s period=10s #success=1 #failure=10
    Environment:
      GF_SECURITY_ADMIN_USER:      <set to the key 'admin-user' in secret 'grafana'>      Optional: false
      GF_SECURITY_ADMIN_PASSWORD:  <set to the key 'admin-password' in secret 'grafana'>  Optional: false
    Mounts:
      /etc/grafana/ from config (rw)
      /etc/grafana/provisioning/dashboards/ from dashboardproviders (rw)
      /etc/grafana/provisioning/datasources/ from datasources (rw)
      /var/lib/grafana from grafana-data (rw)
      /var/lib/grafana/dashboards from dashboards (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from grafana-token-6d8cs (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             False 
  ContainersReady   False 
  PodScheduled      True 
Volumes:
  grafana-data:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  grafana-data-grafana-0
    ReadOnly:   false
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-ini
    Optional:  false
  datasources:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-datasources
    Optional:  false
  dashboardproviders:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-dashboardproviders
    Optional:  false
  dashboards:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-dashboards
    Optional:  false
  grafana-token-6d8cs:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  grafana-token-6d8cs
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age                From               Message
  ----     ------            ----               ----               -------
  Normal   Scheduled         88s                default-scheduler  Successfully assigned monitoring/grafana-0 to minikube
  Warning  FailedScheduling  88s                default-scheduler  0/1 nodes are available: 1 pod has unbound immediate PersistentVolumeClaims.
  Normal   Pulled            81s                kubelet            Container image "debian:9" already present on machine
  Normal   Created           81s                kubelet            Created container init-chmod-data
  Normal   Started           80s                kubelet            Started container init-chmod-data
  Warning  Failed            69s                kubelet            Failed to pull image "grafana/grafana:6.0.1": rpc error: code = Unknown desc = Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 192.168.64.1:53: read udp 192.168.64.3:59961->192.168.64.1:53: i/o timeout
  Warning  Failed            69s                kubelet            Error: ErrImagePull
  Normal   BackOff           68s                kubelet            Back-off pulling image "grafana/grafana:6.0.1"
  Warning  Failed            68s                kubelet            Error: ImagePullBackOff
  Normal   Pulling           56s (x2 over 79s)  kubelet            Pulling image "grafana/grafana:6.0.1"
  Normal   Pulled            52s                kubelet            Successfully pulled image "grafana/grafana:6.0.1" in 4.080886462s
  Normal   Created           52s                kubelet            Created container grafana
  Normal   Started           51s                kubelet            Started container grafana
  Warning  Unhealthy         51s                kubelet            Liveness probe failed: Get "http://172.17.0.4:3000/api/health": dial tcp 172.17.0.4:3000: connect: connection refused
arames625@INSML-3YMD6M prometheus-sts % kubectl logs grafana-0 -n monitoring
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting Grafana" logger=server version=6.0.1 commit=0c44a04 branch=HEAD compiled=2019-03-06T14:21:49+0000
t=2021-03-18T03:02:26+0000 lvl=info msg="Config loaded from" logger=settings file=/usr/share/grafana/conf/defaults.ini
t=2021-03-18T03:02:26+0000 lvl=info msg="Config loaded from" logger=settings file=/etc/grafana/grafana.ini
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.data=/var/lib/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.logs=/var/log/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.plugins=/var/lib/grafana/plugins"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.provisioning=/etc/grafana/provisioning"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.log.mode=console"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_DATA=/var/lib/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_LOGS=/var/log/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_PLUGINS=/var/lib/grafana/plugins"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_PROVISIONING=/etc/grafana/provisioning"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_SECURITY_ADMIN_USER=admin"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_SECURITY_ADMIN_PASSWORD=*********"
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Home" logger=settings path=/usr/share/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Data" logger=settings path=/var/lib/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Logs" logger=settings path=/var/log/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Plugins" logger=settings path=/var/lib/grafana/plugins
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Provisioning" logger=settings path=/etc/grafana/provisioning
t=2021-03-18T03:02:26+0000 lvl=info msg="App mode production" logger=settings
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing HTTPServer" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing SqlStore" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Connecting to DB" logger=sqlstore dbtype=sqlite3
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting DB migration" logger=migrator
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing InternalMetricsService" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing SearchService" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing PluginManager" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting plugin search" logger=plugins
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing RenderingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing AlertingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing DatasourceCacheService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing HooksService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing LoginService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing QuotaService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing ServerLockService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing UsageStatsService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing UserAuthTokenService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing CleanUpService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing NotificationService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing ProvisioningService" logger=server
t=2021-03-18T03:02:28+0000 lvl=eror msg="Can't read alert notification provisioning files from directory" logger=provisioning.notifiers path=/etc/grafana/provisioning/notifiers error="open /etc/grafana/provisioning/notifiers: no such file or directory"
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing TracingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="cleanup of expired auth tokens done" logger=auth count=0
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing Stream Manager"
t=2021-03-18T03:02:28+0000 lvl=info msg="HTTP Server Listen" logger=http.server address=0.0.0.0:3000 protocol=http subUrl= socket=
arames625@INSML-3YMD6M prometheus-sts % kubectl get all -n monitoring       
NAME               READY   STATUS    RESTARTS   AGE
pod/grafana-0      1/1     Running   0          111s
pod/prometheus-0   1/1     Running   0          2m59s

NAME                 TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/grafana      NodePort   10.99.163.8    <none>        3000:30300/TCP   106s
service/prometheus   NodePort   10.103.149.8   <none>        9090:30900/TCP   2m36s

NAME                          READY   AGE
statefulset.apps/grafana      1/1     111s
statefulset.apps/prometheus   1/1     2m59s
arames625@INSML-3YMD6M prometheus-sts % kubectl describe po grafana-0 -n monitoring
Name:         grafana-0
Namespace:    monitoring
Priority:     0
Node:         minikube/192.168.64.3
Start Time:   Thu, 18 Mar 2021 08:31:44 +0530
Labels:       controller-revision-hash=grafana-7548658bdb
              k8s-app=grafana
              statefulset.kubernetes.io/pod-name=grafana-0
Annotations:  <none>
Status:       Running
IP:           172.17.0.4
IPs:
  IP:           172.17.0.4
Controlled By:  StatefulSet/grafana
Init Containers:
  init-chmod-data:
    Container ID:  docker://a7315b97b4263818b06dad6cf91e8444143279ae6e10a78294846ed8e4268901
    Image:         debian:9
    Image ID:      docker-pullable://debian@sha256:ca2af3c25b43f185cc86cc2038a217d9b4cdbb4d47adbcfe5d25e04c1d75e1d9
    Port:          <none>
    Host Port:     <none>
    Command:
      chmod
      777
      /var/lib/grafana
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Thu, 18 Mar 2021 08:31:52 +0530
      Finished:     Thu, 18 Mar 2021 08:31:52 +0530
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /var/lib/grafana from grafana-data (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from grafana-token-6d8cs (ro)
Containers:
  grafana:
    Container ID:   docker://55b0a968c2cbae3c8707be91c6bcc8e728a1d9dfc4886f6ee5a0eba5d0885cae
    Image:          grafana/grafana:6.0.1
    Image ID:       docker-pullable://grafana/grafana@sha256:dcfccebd894c3596827ef7ef5e0a69a669a3a761a19e382f896f11531f45b8fb
    Ports:          80/TCP, 3000/TCP
    Host Ports:     0/TCP, 0/TCP
    State:          Running
      Started:      Thu, 18 Mar 2021 08:32:21 +0530
    Ready:          True
    Restart Count:  0
    Limits:
      cpu:     50m
      memory:  100Mi
    Requests:
      cpu:      50m
      memory:   100Mi
    Liveness:   http-get http://:3000/api/health delay=0s timeout=1s period=10s #success=1 #failure=3
    Readiness:  http-get http://:3000/api/health delay=60s timeout=30s period=10s #success=1 #failure=10
    Environment:
      GF_SECURITY_ADMIN_USER:      <set to the key 'admin-user' in secret 'grafana'>      Optional: false
      GF_SECURITY_ADMIN_PASSWORD:  <set to the key 'admin-password' in secret 'grafana'>  Optional: false
    Mounts:
      /etc/grafana/ from config (rw)
      /etc/grafana/provisioning/dashboards/ from dashboardproviders (rw)
      /etc/grafana/provisioning/datasources/ from datasources (rw)
      /var/lib/grafana from grafana-data (rw)
      /var/lib/grafana/dashboards from dashboards (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from grafana-token-6d8cs (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  grafana-data:
    Type:       PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
    ClaimName:  grafana-data-grafana-0
    ReadOnly:   false
  config:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-ini
    Optional:  false
  datasources:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-datasources
    Optional:  false
  dashboardproviders:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-dashboardproviders
    Optional:  false
  dashboards:
    Type:      ConfigMap (a volume populated by a ConfigMap)
    Name:      grafana-dashboards
    Optional:  false
  grafana-token-6d8cs:
    Type:        Secret (a volume populated by a Secret)
    SecretName:  grafana-token-6d8cs
    Optional:    false
QoS Class:       Burstable
Node-Selectors:  <none>
Tolerations:     node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                 node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age                 From               Message
  ----     ------            ----                ----               -------
  Normal   Scheduled         114s                default-scheduler  Successfully assigned monitoring/grafana-0 to minikube
  Warning  FailedScheduling  114s                default-scheduler  0/1 nodes are available: 1 pod has unbound immediate PersistentVolumeClaims.
  Normal   Pulled            108s                kubelet            Container image "debian:9" already present on machine
  Normal   Created           108s                kubelet            Created container init-chmod-data
  Normal   Started           107s                kubelet            Started container init-chmod-data
  Warning  Failed            96s                 kubelet            Failed to pull image "grafana/grafana:6.0.1": rpc error: code = Unknown desc = Error response from daemon: Get https://registry-1.docker.io/v2/: dial tcp: lookup registry-1.docker.io on 192.168.64.1:53: read udp 192.168.64.3:59961->192.168.64.1:53: i/o timeout
  Warning  Failed            96s                 kubelet            Error: ErrImagePull
  Normal   BackOff           95s                 kubelet            Back-off pulling image "grafana/grafana:6.0.1"
  Warning  Failed            95s                 kubelet            Error: ImagePullBackOff
  Normal   Pulling           83s (x2 over 106s)  kubelet            Pulling image "grafana/grafana:6.0.1"
  Normal   Pulled            79s                 kubelet            Successfully pulled image "grafana/grafana:6.0.1" in 4.080886462s
  Normal   Created           79s                 kubelet            Created container grafana
  Normal   Started           78s                 kubelet            Started container grafana
  Warning  Unhealthy         78s                 kubelet            Liveness probe failed: Get "http://172.17.0.4:3000/api/health": dial tcp 172.17.0.4:3000: connect: connection refused
arames625@INSML-3YMD6M prometheus-sts % kubectl logs grafana-0 -n monitoring       
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting Grafana" logger=server version=6.0.1 commit=0c44a04 branch=HEAD compiled=2019-03-06T14:21:49+0000
t=2021-03-18T03:02:26+0000 lvl=info msg="Config loaded from" logger=settings file=/usr/share/grafana/conf/defaults.ini
t=2021-03-18T03:02:26+0000 lvl=info msg="Config loaded from" logger=settings file=/etc/grafana/grafana.ini
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.data=/var/lib/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.logs=/var/log/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.plugins=/var/lib/grafana/plugins"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.paths.provisioning=/etc/grafana/provisioning"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from command line" logger=settings arg="default.log.mode=console"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_DATA=/var/lib/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_LOGS=/var/log/grafana"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_PLUGINS=/var/lib/grafana/plugins"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_PATHS_PROVISIONING=/etc/grafana/provisioning"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_SECURITY_ADMIN_USER=admin"
t=2021-03-18T03:02:26+0000 lvl=info msg="Config overridden from Environment variable" logger=settings var="GF_SECURITY_ADMIN_PASSWORD=*********"
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Home" logger=settings path=/usr/share/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Data" logger=settings path=/var/lib/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Logs" logger=settings path=/var/log/grafana
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Plugins" logger=settings path=/var/lib/grafana/plugins
t=2021-03-18T03:02:26+0000 lvl=info msg="Path Provisioning" logger=settings path=/etc/grafana/provisioning
t=2021-03-18T03:02:26+0000 lvl=info msg="App mode production" logger=settings
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing HTTPServer" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing SqlStore" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Connecting to DB" logger=sqlstore dbtype=sqlite3
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting DB migration" logger=migrator
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing InternalMetricsService" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing SearchService" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Initializing PluginManager" logger=server
t=2021-03-18T03:02:26+0000 lvl=info msg="Starting plugin search" logger=plugins
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing RenderingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing AlertingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing DatasourceCacheService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing HooksService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing LoginService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing QuotaService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing ServerLockService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing UsageStatsService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing UserAuthTokenService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing CleanUpService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing NotificationService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing ProvisioningService" logger=server
t=2021-03-18T03:02:28+0000 lvl=eror msg="Can't read alert notification provisioning files from directory" logger=provisioning.notifiers path=/etc/grafana/provisioning/notifiers error="open /etc/grafana/provisioning/notifiers: no such file or directory"
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing TracingService" logger=server
t=2021-03-18T03:02:28+0000 lvl=info msg="cleanup of expired auth tokens done" logger=auth count=0
t=2021-03-18T03:02:28+0000 lvl=info msg="Initializing Stream Manager"
t=2021-03-18T03:02:28+0000 lvl=info msg="HTTP Server Listen" logger=http.server address=0.0.0.0:3000 protocol=http subUrl= socket=
arames625@INSML-3YMD6M prometheus-sts % minikube service list
|-------------|------------|-----------------|---------------------------|
|  NAMESPACE  |    NAME    |   TARGET PORT   |            URL            |
|-------------|------------|-----------------|---------------------------|
| default     | kubernetes | No node port    |
| kube-system | kube-dns   | No node port    |
| monitoring  | grafana    | grafana/3000    | http://192.168.64.3:30300 |
| monitoring  | prometheus | prometheus/9090 | http://192.168.64.3:30900 |
|-------------|------------|-----------------|---------------------------|
arames625@INSML-3YMD6M prometheus-sts % 
