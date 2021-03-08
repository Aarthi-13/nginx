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







[node1 ~]$ git clone https://github.com/Aarthi-13/nginx.git
Cloning into 'nginx'...
remote: Enumerating objects: 104, done.
remote: Counting objects: 100% (104/104), done.
remote: Compressing objects: 100% (101/101), done.
remote: Total 104 (delta 46), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (104/104), 33.20 KiB | 0 bytes/s, done.
Resolving deltas: 100% (46/46), done.
[node1 ~]$ ls
anaconda-ks.cfg  nginx
[node1 ~]$ cd nginx
[node1 nginx]$ ls
README.md  nginx-cm.yaml  nginx-po.yaml  nginx-pv.yaml  nginx-pvc.yaml  nginx-svc.yaml  prom-sf.yaml  prom-sv.yaml
[node1 nginx]$ git checkout DaemonSet
Branch DaemonSet set up to track remote branch DaemonSet from origin.
Switched to a new branch 'DaemonSet'
[node1 nginx]$ git branch
* DaemonSet
  main
[node1 nginx]$ ls
README.md  README_DS.md  ds-conf.yaml  nginx-cm.yaml  nginx-ds.yaml  nginx-po.yaml  nginx-svc.yaml
[node1 nginx]$ kubectl create -f ds-conf.yaml
configmap/fluentd-es-config-v0.1.0 created
[node1 nginx]$ kubectl create -f nginx-ds.yaml
daemonset.apps/fluentd-ds created
[node1 nginx]$
[node1 nginx]$ kubectl get po
NAME               READY   STATUS              RESTARTS   AGE
fluentd-ds-dscrz   0/1     ContainerCreating   0          22s
fluentd-ds-qblvc   0/1     ContainerCreating   0          22s
fluentd-ds-x8rkg   0/1     ContainerCreating   0          22s
[node1 nginx]$ kubectl get po
NAME               READY   STATUS    RESTARTS   AGE
fluentd-ds-dscrz   1/1     Running   0          28s
fluentd-ds-qblvc   1/1     Running   0          28s
fluentd-ds-x8rkg   1/1     Running   0          28s
[node1 nginx]$
[node1 nginx]$
[node1 nginx]$ kubectl get pods -o wide
NAME               READY   STATUS    RESTARTS   AGE   IP         NODE    NOMINATED NODE   READINESS GATES
fluentd-ds-dscrz   1/1     Running   0          61s   10.5.3.2   node4   <none>           <none>
fluentd-ds-qblvc   1/1     Running   0          61s   10.5.2.2   node3   <none>           <none>
fluentd-ds-x8rkg   1/1     Running   0          61s   10.5.1.2   node2   <none>           <none>
[node1 nginx]$
[node1 nginx]$
[node1 nginx]$ kubectl logs fluentd-ds-dscrz
2021-03-08 00:58:19 +0000 [info]: reading config file path="/etc/td-agent/td-agent.conf"
2021-03-08 00:58:19 +0000 [info]: starting fluentd-0.12.29
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-mixin-config-placeholders' version '0.4.0'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-mixin-plaintextformatter' version '0.2.6'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-docker_metadata_filter' version '0.1.3'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-elasticsearch' version '1.5.0'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-kafka' version '0.3.1'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-kubernetes_metadata_filter' version '0.24.0'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-mongo' version '0.7.15'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-rewrite-tag-filter' version '1.5.5'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-s3' version '0.7.1'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-scribe' version '0.10.14'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-td' version '0.10.29'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-td-monitoring' version '0.2.2'
2021-03-08 00:58:19 +0000 [info]: gem 'fluent-plugin-webhdfs' version '0.4.2'
2021-03-08 00:58:19 +0000 [info]: gem 'fluentd' version '0.12.29'
2021-03-08 00:58:19 +0000 [info]: adding match pattern="fluent.**" type="null"
2021-03-08 00:58:19 +0000 [info]: adding match pattern="**" type="elasticsearch"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: adding source type="tail"
2021-03-08 00:58:19 +0000 [info]: using configuration file: <ROOT>
  <match fluent.**>
    type null
  </match>
  <source>
    type tail
    path /var/log/containers/*.log
    pos_file /var/log/es-containers.log.pos
    time_format %Y-%m-%dT%H:%M:%S.%NZ
    tag kubernetes.*
    format json
    read_from_head true
  </source>
  <source>
    type tail
    format /^(?<time>[^ ]* [^ ,]*)[^\[]*\[[^\]]*\]\[(?<severity>[^ \]]*) *\] (?<message>.*)$/
    time_format %Y-%m-%d %H:%M:%S
    path /var/log/salt/minion
    pos_file /var/log/es-salt.pos
    tag salt
  </source>
  <source>
    type tail
    format syslog
    path /var/log/startupscript.log
    pos_file /var/log/es-startupscript.log.pos
    tag startupscript
  </source>
  <source>
    type tail
    format /^time="(?<time>[^)]*)" level=(?<severity>[^ ]*) msg="(?<message>[^"]*)"( err="(?<error>[^"]*)")?( statusCode=($<status_code>\d+))?/
    path /var/log/docker.log
    pos_file /var/log/es-docker.log.pos
    tag docker
  </source>
  <source>
    type tail
    format none
    path /var/log/etcd.log
    pos_file /var/log/es-etcd.log.pos
    tag etcd
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/kubelet.log
    pos_file /var/log/es-kubelet.log.pos
    tag kubelet
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/kube-proxy.log
    pos_file /var/log/es-kube-proxy.log.pos
    tag kube-proxy
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/kube-apiserver.log
    pos_file /var/log/es-kube-apiserver.log.pos
    tag kube-apiserver
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/kube-controller-manager.log
    pos_file /var/log/es-kube-controller-manager.log.pos
    tag kube-controller-manager
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/kube-scheduler.log
    pos_file /var/log/es-kube-scheduler.log.pos
    tag kube-scheduler
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/rescheduler.log
    pos_file /var/log/es-rescheduler.log.pos
    tag rescheduler
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/glbc.log
    pos_file /var/log/es-glbc.log.pos
    tag glbc
  </source>
  <source>
    type tail
    format multiline
    multiline_flush_interval 5s
    format_firstline /^\w\d{4}/
    format1 /^(?<severity>\w)(?<time>\d{4} [^\s]*)\s+(?<pid>\d+)\s+(?<source>[^ \]]+)\] (?<message>.*)/
    time_format %m%d %H:%M:%S.%N
    path /var/log/cluster-autoscaler.log
    pos_file /var/log/es-cluster-autoscaler.log.pos
    tag cluster-autoscaler
  </source>
  <match **>
    type elasticsearch
    log_level info
    include_tag_key true
    host elasticsearch-logging
    port 9200
    logstash_format true
    buffer_chunk_limit 2M
    buffer_queue_limit 32
    flush_interval 5s
    max_retry_wait 30
    disable_retry_limit
    num_threads 8
  </match>
</ROOT>
2021-03-08 00:58:19 +0000 [info]: following tail of /var/log/containers/kube-proxy-x85rl_kube-system_kube-proxy-482f1a5e146fcfc4470ac4ac2a50290e1dd1e5ba50647ad6d5ee620465536cab.log
2021-03-08 00:58:19 +0000 [info]: following tail of /var/log/containers/kube-router-hr7rn_kube-system_install-cni-d9033aafb37353b98c4c30cfde03b7a5094e9c453127edc379df6d2a3db3a7f4.log
2021-03-08 00:58:19 +0000 [info]: following tail of /var/log/containers/kube-router-hr7rn_kube-system_kube-router-92eb6bceb4cb1c257965390f2f7a267aa8eb16c9b90ec4b70fd20005f9f32848.log
2021-03-08 00:58:19 +0000 [info]: following tail of /var/log/containers/fluentd-ds-dscrz_default_fluentd-9d80ad7c0c767d8899403876a9b5543ab74de74edf20683df78b2dc585741cf3.log
2021-03-08 00:58:25 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:58:26 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluent-plugin-elasticsearch-1.5.0/lib/fluent/plugin/out_elasticsearch.rb:122:in `client'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluent-plugin-elasticsearch-1.5.0/lib/fluent/plugin/out_elasticsearch.rb:279:in `rescue in send'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluent-plugin-elasticsearch-1.5.0/lib/fluent/plugin/out_elasticsearch.rb:277:in `send'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluent-plugin-elasticsearch-1.5.0/lib/fluent/plugin/out_elasticsearch.rb:271:in `write'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluentd-0.12.29/lib/fluent/buffer.rb:354:in `write_chunk'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluentd-0.12.29/lib/fluent/buffer.rb:333:in `pop'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluentd-0.12.29/lib/fluent/output.rb:338:in `try_flush'
  2021-03-08 00:58:25 +0000 [warn]: /opt/td-agent/embedded/lib/ruby/gems/2.1.0/gems/fluentd-0.12.29/lib/fluent/output.rb:149:in `run'
2021-03-08 00:58:26 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:58:28 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:26 +0000 [warn]: suppressed same stacktrace
2021-03-08 00:58:28 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:58:32 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:28 +0000 [warn]: suppressed same stacktrace
2021-03-08 00:58:32 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:58:39 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:32 +0000 [warn]: suppressed same stacktrace
2021-03-08 00:58:39 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:58:56 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:39 +0000 [warn]: suppressed same stacktrace
2021-03-08 00:58:56 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:59:26 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:58:56 +0000 [warn]: suppressed same stacktrace
2021-03-08 00:59:26 +0000 [warn]: temporarily failed to flush the buffer. next_retry=2021-03-08 00:59:56 +0000 error_class="Fluent::ElasticsearchOutput::ConnectionFailure" error="Can not reach Elasticsearch cluster ({:host=>\"elasticsearch-logging\", :port=>9200, :scheme=>\"http\"})!" plugin_id="object:3ff064db76f0"
  2021-03-08 00:59:26 +0000 [warn]: suppressed same stacktrace
[node1 nginx]$
