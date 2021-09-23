## efk
日志收集平台

## 命名空间
`执行logging-namespace.yaml文件`
```
kubectl create -f logging-namespace.yaml
```

## 部署服务
`执行每个目录里面的yaml文件`
```
kubectl create -f ./counter
kubectl create -f ./fluentd
kubectl create -f ./head
kubectl create -f ./elasticsearch/nfs
kubectl create -f ./elasticsearch
kubectl create -f ./kibana
```
## 查看执行结果
`查看pod`
>[root@k8s-master ~]# kubectl get -n logging pod
```
NAME                                  READY   STATUS    RESTARTS   AGE
elasticsearch-head-7c795d9b46-8j2gn   1/1     Running   0          6d5h
es-cluster-0                          1/1     Running   1          6d3h
es-cluster-1                          1/1     Running   1          6d3h
es-cluster-2                          1/1     Running   1          6d3h
fluentd-es-jvp9w                      1/1     Running   2          6d3h
kibana-685bd77cb8-b9kw2               1/1     Running   0          6d3h
```
`查看config`
>[root@k8s-master ~]# kubectl get -n logging configmaps 
```
NAME                      DATA   AGE
fluentd-config            5      29h
fluentd-config-database   2      29h
```
`查看pvc`
>[root@k8s-master ~]# kubectl get -n logging pvc
```
NAME                STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS        AGE
data-es-cluster-0   Bound    pvc-913d42ef-a09e-4370-b415-48fa66b29913   10Gi       RWO            elasticsearch-nfs   6d3h
data-es-cluster-1   Bound    pvc-f35426a8-6736-4730-bf58-743978e6ec48   10Gi       RWO            elasticsearch-nfs   6d3h
data-es-cluster-2   Bound    pvc-b1edc349-0895-43fc-bc7d-6d0a188b0a34   10Gi       RWO            elasticsearch-nfs   6d3h
```
`查看service`
>[root@k8s-master ~]# kubectl get -n logging service
```
NAME                 TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)             AGE
elasticsearch        ClusterIP   10.103.28.99   <none>        9200/TCP,9300/TCP   29h
elasticsearch-head   ClusterIP   10.98.249.57   <none>        9100/TCP            6d5h
kibana               ClusterIP   10.96.24.100   <none>        5601/TCP            6d3h
```
`查看storageclasses`
>[root@k8s-master ~]# kubectl get storageclasses.storage.k8s.io 
```
NAME                PROVISIONER      RECLAIMPOLICY   VOLUMEBINDINGMODE   ALLOWVOLUMEEXPANSION   AGE
elasticsearch-nfs   fuseim.pri/ifs   Delete          Immediate           false                  6d4h
```
`查看ingress`
>[root@k8s-master ~]# kubectl get -n logging ingress
```
NAME                 CLASS    HOSTS                       ADDRESS       PORTS   AGE
elasticsearch        <none>   elasticsearch.liangzy.com   10.99.99.42   80      6d3h
elasticsearch-head   <none>   head.liangzy.com            10.99.99.42   80      6d5h
kibana               <none>   kibana.liangzy.com          10.99.99.42   80      6d3h
```
