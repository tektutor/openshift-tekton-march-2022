# CI/CD with TekTon on OpenShift
For training/consulting/coaching, you may reach me
<pre>
    jegan@tektutor.org
    +91 822-000-5626 (WhatsApp)
</pre>

## Don't miss a Kubernetes/OpenShift article ( Hit the clap button a few times and follow the author in medium )
https://medium.com/tektutor

### My Articles
#### Setting up a 3 node Kubernetes Cluster on your local machine

```
https://medium.com/tektutor/kubernetes-3-node-cluster-setup-50943378be41
```

#### Using MetalLB Load Balancer in bare-metal Kubernetes Cluster

```
https://medium.com/tektutor/using-metal-lb-on-a-bare-metal-onprem-kubernetes-setup-6d036af1d20c
```

#### Using Nginx Ingress Controller in bare-metal Kubernetes Cluster

```
https://medium.com/tektutor/using-nginx-ingress-controller-in-kubernetes-bare-metal-setup-890eb4e7772
```

#### Deploying Stateful applications in a Kubernetes Cluster

```
https://medium.com/tektutor/deploying-stateful-applications-in-kubernetes-8ffd46920b55
```

#### Understand the different between Container Engine and Container Runtime
```
https://medium.com/tektutor/container-engine-vs-container-runtime-667a99042f3
```


## What really happens when we deploy an application in OpenShift/Kubernetes?
Let say we wish to create a deployment for sping-ms application from the below GitHub Repo.

```
oc new-app https://github.com/tektutor/spring-ms.git
```

1. oc client tool will send a REST API Request to API Server to create a Deployment in master node
2. The API Server in the master node will receive the request from oc client and will create a Deployment definition in the etcd datastore.
3. When a new Deployment is added to the etcd, it triggers a "New Deployment Created" kind of event.
4. The Deployment Controller running in the master node receives this event and retrieves the information from the event and it inturn sends a REST API request to the API Server to create a ReplicaSet.
5. Once the API Server receives the request from Deployment Controller, it creates a ReplicaSet definition in the etcd key/value datastore.
6. As soon as new ReplicaSet is added to the etcd, API Server broadcasts a "New ReplicaSet Created" kind of event.
7. The ReplicaSet Controller receives this event, and will send a request to the API Server to create the x number of Pod definition as mention in the "New ReplicaSet created" event.
8. The API Server will create x number of Pod definitions as requested by the ReplicaSet Controller in the etcd datastore.
9. This will again trigger "New Pod Created" kind of event.
10. The Scheduler which is running in the master node, receives this event and identifies a healthy node that can host the Pod containers. The Scheduler then, sends this scheduling recommendation/request to the API Server.
11. Now the API Server will update the node details in the existing Pod definition(s) that are stored in the etcd datastore.
12. This will trigger "New Pod Created" sort of events, which are received by the kubelet Kubernetes Agent running on the respective worker nodes.
13. The kubelet will then will check if the container image is present on the node where kubelet is running, if it is not there, then kubelet will request the Container Enginer(Docker) to download that image.
14. Once the Container Image is downloaded, kubelet creates a pause container and the application containers mentioned in the Pod definition.
15. The Kubelet agent periodically monitors the health of the containers created by kubelet and keeps giving heart-beat kind of periodically updates to the API Server by making REST API calls.
16. The API Server will update the status of the Pod and the respective container status in the etcd datastore.

## ⛹️‍♀️ Lab - Checking who has logged in via CLI
```
oc whoami
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc whoami</b>
kube:admin
</pre>

## ⛹️‍♀️ Lab - List pods in all namespaces as kubeadmin
```
kubectl get po --all-namespaces
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>kubectl get po --all-namespaces</b>
NAMESPACE                                          NAME                                                         READY   STATUS      RESTARTS          AGE
jegan                                              hello-spring-boot-1-build                                    0/1     Completed   0                 151m
jegan                                              hello-spring-boot-74867d477b-7nwn7                           1/1     Running     0                 147m
openshift-apiserver-operator                       openshift-apiserver-operator-7c489d54cb-qprqf                1/1     Running     52 (6h17m ago)    27d
openshift-apiserver                                apiserver-8547db8969-fr4ks                                   2/2     Running     8                 27d
openshift-apiserver                                apiserver-8547db8969-pmqqn                                   2/2     Running     8                 27d
openshift-apiserver                                apiserver-8547db8969-vdlmq                                   2/2     Running     2                 2d4h
openshift-authentication-operator                  authentication-operator-8476b89b69-h429d                     1/1     Running     86 (6h15m ago)    27d
openshift-authentication                           oauth-openshift-5c44f4cdf9-jbh7j                             1/1     Running     1                 2d4h
openshift-authentication                           oauth-openshift-5c44f4cdf9-lr5st                             1/1     Running     1                 2d14h
openshift-authentication                           oauth-openshift-5c44f4cdf9-nvv5j                             1/1     Running     1                 2d14h
openshift-cloud-controller-manager-operator        cluster-cloud-controller-manager-operator-6d64494fcd-k2dqh   2/2     Running     523 (51m ago)     27d
openshift-cloud-credential-operator                cloud-credential-operator-575668d99-qzhd5                    2/2     Running     22                27d
openshift-cluster-machine-approver                 machine-approver-7dbfcc4f8b-k2kk5                            2/2     Running     14 (41h ago)      27d
openshift-cluster-node-tuning-operator             cluster-node-tuning-operator-8555b6c84-5prxj                 1/1     Running     29 (6h17m ago)    27d
openshift-cluster-node-tuning-operator             tuned-cxbwm                                                  1/1     Running     4                 27d
openshift-cluster-node-tuning-operator             tuned-h94rx                                                  1/1     Running     4                 27d
openshift-cluster-node-tuning-operator             tuned-ktsr7                                                  1/1     Running     4                 27d
openshift-cluster-node-tuning-operator             tuned-l78z6                                                  1/1     Running     4                 27d
openshift-cluster-node-tuning-operator             tuned-nzlg7                                                  1/1     Running     4                 27d
openshift-cluster-samples-operator                 cluster-samples-operator-5597dbd778-vbpbp                    2/2     Running     18 (14h ago)      26d
openshift-cluster-storage-operator                 cluster-storage-operator-697d9ffb9b-5vjjb                    1/1     Running     56 (6h17m ago)    27d
openshift-cluster-storage-operator                 csi-snapshot-controller-6bc88bf56b-fs5qg                     1/1     Running     15                26d
openshift-cluster-storage-operator                 csi-snapshot-controller-6bc88bf56b-jhtwn                     1/1     Running     23 (6h17m ago)    27d
openshift-cluster-storage-operator                 csi-snapshot-controller-operator-5744b8c9bc-qgxbz            1/1     Running     53 (6h17m ago)    27d
openshift-cluster-storage-operator                 csi-snapshot-webhook-6b7c666b57-2f69f                        1/1     Running     4                 26d
openshift-cluster-storage-operator                 csi-snapshot-webhook-6b7c666b57-qj455                        1/1     Running     4                 27d
openshift-cluster-version                          cluster-version-operator-5945f959cb-qchxv                    1/1     Running     50 (60m ago)      27d
openshift-config-operator                          openshift-config-operator-b8cf5ff77-d6p9d                    1/1     Running     105 (6h16m ago)   27d
openshift-console-operator                         console-operator-696cbf456f-cht8t                            1/1     Running     130 (6h16m ago)   26d
openshift-console                                  console-7847d44f9f-4wprr                                     1/1     Running     4                 27d
openshift-console                                  console-7847d44f9f-5rr6n                                     1/1     Running     4                 27d
openshift-console                                  downloads-6c87455f65-86qq5                                   1/1     Running     0                 6h3m
openshift-console                                  downloads-6c87455f65-jhkzd                                   1/1     Running     1                 17d
openshift-controller-manager-operator              openshift-controller-manager-operator-8fd8f7f7d-dn8ln        1/1     Running     60 (6h17m ago)    27d
openshift-controller-manager                       controller-manager-99st9                                     1/1     Running     5 (6h17m ago)     11d
openshift-controller-manager                       controller-manager-d88tq                                     1/1     Running     6 (23h ago)       11d
openshift-controller-manager                       controller-manager-h8htl                                     1/1     Running     10 (31h ago)      11d
openshift-dns-operator                             dns-operator-bd878d449-75vnz                                 2/2     Running     8                 27d
openshift-dns                                      dns-default-dhtj9                                            2/2     Running     8                 27d
openshift-dns                                      dns-default-fntxh                                            2/2     Running     8                 27d
openshift-dns                                      dns-default-mvfg8                                            2/2     Running     8                 27d
openshift-dns                                      dns-default-pn8tk                                            2/2     Running     8                 27d
openshift-dns                                      dns-default-pqxcn                                            2/2     Running     8                 27d
openshift-dns                                      node-resolver-46dqj                                          1/1     Running     4                 27d
openshift-dns                                      node-resolver-hvvxh                                          1/1     Running     4                 27d
openshift-dns                                      node-resolver-psl88                                          1/1     Running     4                 27d
openshift-dns                                      node-resolver-vtwks                                          1/1     Running     4                 27d
openshift-dns                                      node-resolver-xglgq                                          1/1     Running     4                 27d
openshift-etcd-operator                            etcd-operator-69589d779c-rxd77                               1/1     Running     62 (6h17m ago)    27d
openshift-etcd                                     etcd-master-1.tektutor.tektutor.org                          4/4     Running     16                26d
openshift-etcd                                     etcd-master-2.tektutor.tektutor.org                          4/4     Running     16                26d
openshift-etcd                                     etcd-master-3.tektutor.tektutor.org                          4/4     Running     16                26d
openshift-etcd                                     etcd-quorum-guard-fbbfd7767-gmwst                            1/1     Running     4                 27d
openshift-etcd                                     etcd-quorum-guard-fbbfd7767-llcjt                            1/1     Running     4                 27d
openshift-etcd                                     etcd-quorum-guard-fbbfd7767-mwwbl                            1/1     Running     4                 27d
openshift-etcd                                     installer-2-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-etcd                                     installer-2-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-etcd                                     installer-2-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-etcd                                     installer-3-master-1.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-etcd                                     installer-3-master-2.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-etcd                                     installer-3-master-3.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-image-registry                           cluster-image-registry-operator-88b89c5f6-nns2s              1/1     Running     45 (6h18m ago)    27d
openshift-image-registry                           image-pruner-27463680--1-8vvkg                               0/1     Completed   0                 2d3h
openshift-image-registry                           image-pruner-27466560--1-v7n25                               0/1     Completed   0                 3h5m
openshift-image-registry                           image-registry-6998cfc5c-2fmc2                               1/1     Running     4                 27d
openshift-image-registry                           node-ca-8gqrr                                                1/1     Running     4                 27d
openshift-image-registry                           node-ca-9wjdm                                                1/1     Running     4                 27d
openshift-image-registry                           node-ca-jxn96                                                1/1     Running     4                 27d
openshift-image-registry                           node-ca-n575b                                                1/1     Running     4                 27d
openshift-image-registry                           node-ca-zgh8b                                                1/1     Running     4                 27d
openshift-ingress-canary                           ingress-canary-88cbh                                         1/1     Running     4                 27d
openshift-ingress-canary                           ingress-canary-c6jlr                                         1/1     Running     4                 27d
openshift-ingress-canary                           ingress-canary-cqv86                                         1/1     Running     4                 27d
openshift-ingress-canary                           ingress-canary-hl25p                                         1/1     Running     4                 27d(jegan@tektutor.org)$ oc status
In project jegan on server https://api.tektutor.tektutor.org:6443

deployment/openshift-spring deploys istag/openshift-spring:latest <-
  bc/openshift-spring docker builds https://github.com/tektutor/openshift-spring.git on istag/openjdk:latest 
    build #1 running for 6 seconds
  deployment #1 running for 8 seconds - 0/1 pods growing to 1


1 info identified, use 'oc status --suggest' to see details.

openshift-ingress-canary                           ingress-canary-pn64p                                         1/1     Running     4                 27d
openshift-ingress-operator                         ingress-operator-597f796c69-cxpfp                            2/2     Running     16                27d
openshift-ingress                                  router-default-6776cf78dc-44bk5                              1/1     Running     2 (41h ago)       2d4h
openshift-ingress                                  router-default-6776cf78dc-hl9v2                              1/1     Running     5 (41h ago)       27d
openshift-ingress                                  router-default-6776cf78dc-sqfw7                              1/1     Running     5 (41h ago)       27d
openshift-insights                                 insights-operator-5b54ccbdf6-mzgkv                           1/1     Running     5                 27d
openshift-kube-apiserver-operator                  kube-apiserver-operator-78667c7c77-7cdjp                     1/1     Running     229 (51m ago)     27d
openshift-kube-apiserver                           installer-10-master-1.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-10-master-2.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-10-master-3.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-11-master-1.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-11-master-2.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-11-master-3.tektutor.tektutor.org                  0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-12-master-1.tektutor.tektutor.org                  0/1     Completed   0                 11d
openshift-kube-apiserver                           installer-12-master-2.tektutor.tektutor.org                  0/1     Completed   0                 11d
openshift-kube-apiserver                           installer-12-master-3.tektutor.tektutor.org                  0/1     Completed   0                 11d
openshift-kube-apiserver                           installer-13-master-1.tektutor.tektutor.org                  0/1     Completed   0                 3d14h
openshift-kube-apiserver                           installer-13-master-2.tektutor.tektutor.org                  0/1     Completed   0                 3d14h
openshift-kube-apiserver                           installer-13-master-3.tektutor.tektutor.org                  0/1     Completed   0                 3d14h
openshift-kube-apiserver                           installer-9-master-1.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-9-master-2.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-kube-apiserver                           installer-9-master-3.tektutor.tektutor.org                   0/1     Completed   0                 26d
openshift-kube-apiserver                           kube-apiserver-master-1.tektutor.tektutor.org                5/5     Running     7 (6h17m ago)     3d14h
openshift-kube-apiserver                           kube-apiserver-master-2.tektutor.tektutor.org                5/5     Running     8 (23h ago)       3d14h
openshift-kube-apiserver                           kube-apiserver-master-3.tektutor.tektutor.org                5/5     Running     38                3d14h
openshift-kube-apiserver                           revision-pruner-10-master-1.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-10-master-2.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-10-master-3.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-11-master-1.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-11-master-2.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-11-master-3.tektutor.tektutor.org            0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-12-master-1.tektutor.tektutor.org            0/1     Completed   0                 11d
openshift-kube-apiserver                           revision-pruner-12-master-2.tektutor.tektutor.org            0/1     Completed   0                 11d
openshift-kube-apiserver                           revision-pruner-12-master-3.tektutor.tektutor.org            0/1     Completed   0                 11d
openshift-kube-apiserver                           revision-pruner-13-master-1.tektutor.tektutor.org            0/1     Completed   0                 3d14h
openshift-kube-apiserver                           revision-pruner-13-master-2.tektutor.tektutor.org            0/1     Completed   0                 3d14h
openshift-kube-apiserver                           revision-pruner-13-master-3.tektutor.tektutor.org            0/1     Completed   0                 3d14h
openshift-kube-apiserver                           revision-pruner-9-master-1.tektutor.tektutor.org             0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-9-master-2.tektutor.tektutor.org             0/1     Completed   0                 26d
openshift-kube-apiserver                           revision-pruner-9-master-3.tektutor.tektutor.org             0/1     Completed   0                 26d
openshift-kube-controller-manager-operator         kube-controller-manager-operator-86c8975d97-dc2ld            1/1     Running     232 (51m ago)     27d
openshift-kube-controller-manager                  installer-5-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-5-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-5-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-6-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-6-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-7-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-7-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-8-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-8-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-8-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-9-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-9-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  installer-9-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-controller-manager                  kube-controller-manager-master-1.tektutor.tektutor.org       4/4     Running     142 (6h10m ago)   27d
openshift-kube-controller-manager                  kube-controller-manager-master-2.tektutor.tektutor.org       4/4     Running     127 (6h16m ago)   27d
openshift-kube-controller-manager                  kube-controller-manager-master-3.tektutor.tektutor.org       4/4     Running     298 (49m ago)     27d
openshift-kube-controller-manager                  revision-pruner-7-master-1.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-7-master-2.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-7-master-3.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-8-master-1.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-8-master-2.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-8-master-3.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-9-master-1.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-9-master-2.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-controller-manager                  revision-pruner-9-master-3.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-scheduler-operator                  openshift-kube-scheduler-operator-7698567475-9nwmb           1/1     Running     230 (52m ago)     27d
openshift-kube-scheduler                           installer-4-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-5-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-5-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-5-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-6-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-7-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-7-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-8-master-1.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-8-master-2.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           installer-8-master-3.tektutor.tektutor.org                   0/1     Completed   0                 27d
openshift-kube-scheduler                           openshift-kube-scheduler-master-1.tektutor.tektutor.org      3/3     Running     42 (62m ago)      27d
openshift-kube-scheduler                           openshift-kube-scheduler-master-2.tektutor.tektutor.org      3/3     Running     46 (15h ago)      27d
openshift-kube-scheduler                           openshift-kube-scheduler-master-3.tektutor.tektutor.org      3/3     Running     68 (62m ago)      27d
openshift-kube-scheduler                           revision-pruner-8-master-1.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-scheduler                           revision-pruner-8-master-2.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-scheduler                           revision-pruner-8-master-3.tektutor.tektutor.org             0/1     Completed   0                 27d
openshift-kube-storage-version-migrator-operator   kube-storage-version-migrator-operator-76b6ff6b6-kkv26       1/1     Running     54 (6h17m ago)    27d
openshift-kube-storage-version-migrator            migrator-6c64b9ff9b-rjqgn                                    1/1     Running     0                 6h3m
openshift-machine-api                              cluster-autoscaler-operator-5959856469-pgt82                 2/2     Running     102 (6h16m ago)   27d
openshift-machine-api                              cluster-baremetal-operator-dd95bdc4f-kg6tg                   2/2     Running     12                27d
openshift-machine-api                              machine-api-operator-5d769fdf5d-5tvbc                        2/2     Running     31 (6h17m ago)    27d
openshift-machine-config-operator                  machine-config-controller-5c6595444c-z444h                   1/1     Running     39 (6h17m ago)    26d
openshift-machine-config-operator                  machine-config-daemon-879fc                                  2/2     Running     8                 27d
openshift-machine-config-operator                  machine-config-daemon-j624s                                  2/2     Running     8                 27d
openshift-machine-config-operator                  machine-config-daemon-lclxg                                  2/2     Running     8                 27d
openshift-machine-config-operator                  machine-config-daemon-m6b6n                                  2/2     Running     8                 27d
openshift-machine-config-operator                  machine-config-daemon-wsx68                                  2/2     Running     8                 27d
openshift-machine-config-operator                  machine-config-operator-79fdc9b778-rvhtj                     1/1     Running     40 (6h17m ago)    27d
openshift-machine-config-operator                  machine-config-server-c4hr9                                  1/1     Running     4                 27d
openshift-machine-config-operator                  machine-config-server-zrm2d                                  1/1     Running     4                 27d
openshift-machine-config-operator                  machine-config-server-zzhkh                                  1/1     Running     4                 27d
openshift-marketplace                              certified-operators-f7dqp                                    1/1     Running     0                 4h29m
openshift-marketplace                              community-operators-ss4jd                                    1/1     Running     0                 5h18m
openshift-marketplace                              marketplace-operator-fbd7c964f-fkhrr                         1/1     Running     150 (6h16m ago)   27d
openshift-marketplace                              redhat-marketplace-6zs7r                                     1/1     Running     0                 4h26m
openshift-marketplace                              redhat-operators-4b2rh                                       1/1     Running     0                 4h29m
openshift-monitoring                               alertmanager-main-0                                          5/5     Running     20                27d
openshift-monitoring                               alertmanager-main-1                                          5/5     Running     0                 4h26m
openshift-monitoring                               alertmanager-main-2                                          5/5     Running     20                27d
openshift-monitoring                               cluster-monitoring-operator-7f796c9b9c-9zxlw                 2/2     Running     12                27d
openshift-monitoring                               grafana-654b4cfcbd-42ddf                                     2/2     Running     8                 27d
openshift-monitoring                               kube-state-metrics-5c85546cf8-lzt24                          3/3     Running     12                27d
openshift-monitoring                               node-exporter-4d2s4                                          2/2     Running     8                 27d
openshift-monitoring                               node-exporter-g2mqz                                          2/2     Running     8                 27d
openshift-monitoring                               node-exporter-hzd27                                          2/2     Running     8                 27d
openshift-monitoring                               node-exporter-lhdk7                                          2/2     Running     8                 27d
openshift-monitoring                               node-exporter-r84fh                                          2/2     Running     8                 27d
openshift-monitoring                               openshift-state-metrics-6798dcfdbc-m9n98                     3/3     Running     12                27d
openshift-monitoring                               prometheus-adapter-5f4498bd4d-dhpvw                          1/1     Running     1                 8d
openshift-monitoring                               prometheus-adapter-5f4498bd4d-z7pd5                          1/1     Running     0                 6h
openshift-monitoring                               prometheus-k8s-0                                             7/7     Running     28                27d
openshift-monitoring                               prometheus-k8s-1                                             7/7     Running     0                 5h18m
openshift-monitoring                               prometheus-operator-67f64ccf6d-ggpnm                         2/2     Running     8                 26d
openshift-monitoring                               telemeter-client-7b48949474-mlz4v                            3/3     Running     12                27d
openshift-monitoring                               thanos-querier-65cb85c886-dcbpx                              5/5     Running     0                 6h
openshift-monitoring                               thanos-querier-65cb85c886-pmw65                              5/5     Running     20                27d
openshift-multus                                   multus-5lxsq                                                 1/1     Running     4                 27d
openshift-multus                                   multus-9br9f                                                 1/1     Running     4                 27d
openshift-multus                                   multus-additional-cni-plugins-4bnn2                          1/1     Running     4                 27d
openshift-multus                                   multus-additional-cni-plugins-cfsx2                          1/1     Running     4                 27d
openshift-multus                                   multus-additional-cni-plugins-mdjhk                          1/1     Running     4                 27d
openshift-multus                                   multus-additional-cni-plugins-nn6s8                          1/1     Running     4                 27d
openshift-multus                                   multus-additional-cni-plugins-p55w6                          1/1     Running     4                 27d
openshift-multus                                   multus-admission-controller-5vfc6                            2/2     Running     8                 27d
openshift-multus                                   multus-admission-controller-qdqbn                            2/2     Running     8                 27d
openshift-multus                                   multus-admission-controller-zs55k                            2/2     Running     8                 27d
openshift-multus                                   multus-dr8bs                                                 1/1     Running     4                 27d
openshift-multus                                   multus-kjzmw                                                 1/1     Running     4                 27d
openshift-multus                                   multus-wqtsp                                                 1/1     Running     4                 27d
openshift-multus                                   network-metrics-daemon-9hl8d                                 2/2     Running     8                 27d
openshift-multus                                   network-metrics-daemon-nd8kg                                 2/2     Running     8                 27d
openshift-multus                                   network-metrics-daemon-psksg                                 2/2     Running     8                 27d
openshift-multus                                   network-metrics-daemon-rjb5p                                 2/2     Running     8                 27d
openshift-multus                                   network-metrics-daemon-xvsf4                                 2/2     Running     8                 27d
openshift-network-diagnostics                      network-check-source-799884cb69-pqx5x                        1/1     Running     4                 27d
openshift-network-diagnostics                      network-check-target-4crkx                                   1/1     Running     4                 27d
openshift-network-diagnostics                      network-check-target-7bgh4                                   1/1     Running     4                 27d
openshift-network-diagnostics                      network-check-target-7m4dr                                   1/1     Running     4                 27d
openshift-network-diagnostics                      network-check-target-j4mqq                                   1/1     Running     4                 27d
openshift-network-diagnostics                      network-check-target-zcsb6                                   1/1     Running     4                 27d
openshift-network-operator                         network-operator-5c86c8685c-gv5rp                            1/1     Running     214 (53m ago)     27d
openshift-oauth-apiserver                          apiserver-6465c779fc-jxknp                                   1/1     Running     131 (63m ago)     27d
openshift-oauth-apiserver                          apiserver-6465c779fc-n5vw9                                   1/1     Running     8 (62m ago)       2d4h
openshift-oauth-apiserver                          apiserver-6465c779fc-w89vk                                   1/1     Running     138 (6h17m ago)   27d
openshift-operator-lifecycle-manager               catalog-operator-6d84c58566-wqkvt                            1/1     Running     7                 27d
openshift-operator-lifecycle-manager               collect-profiles-27466710--1-wp2jn                           0/1     Completed   0                 35m
openshift-operator-lifecycle-manager               collect-profiles-27466725--1-d4f7n                           0/1     Completed   0                 20m
openshift-operator-lifecycle-manager               collect-profiles-27466740--1-bdj4t                           0/1     Completed   0                 5m34s
openshift-operator-lifecycle-manager               olm-operator-75c57778d5-6vl27                                1/1     Running     7                 27d
openshift-operator-lifecycle-manager               package-server-manager-59cd88b548-hx68m                      1/1     Running     124 (6h16m ago)   27d
openshift-operator-lifecycle-manager               packageserver-54c7f64856-mk2zt                               1/1     Running     21                27d
openshift-operator-lifecycle-manager               packageserver-54c7f64856-tqmrg                               1/1     Running     23                26d
openshift-sdn                                      sdn-7wc4r                                                    2/2     Running     8                 27d
openshift-sdn                                      sdn-8nth5                                                    2/2     Running     8                 27d
openshift-sdn                                      sdn-bvfm5                                                    2/2     Running     8                 27d
openshift-sdn                                      sdn-controller-45s6v                                         1/1     Running     18                27d
openshift-sdn                                      sdn-controller-hzksl                                         1/1     Running     20                27d
openshift-sdn                                      sdn-controller-rln9j                                         1/1     Running     20 (62m ago)      27d
openshift-sdn                                      sdn-kf6z4                                                    2/2     Running     8                 27d
openshift-sdn                                      sdn-lscw7                                                    2/2     Running     8                 27d
openshift-service-ca-operator                      service-ca-operator-68c775c9b7-l4slc                         1/1     Running     51 (6h17m ago)    27d
openshift-service-ca                               service-ca-fb75f5444-x9m9x                                   1/1     Running     59 (6h17m ago)    27d
tekton-pipelines                                   tekton-pipelines-controller-6cd76b8667-pnbvk                 1/1     Running     0                 6h
tekton-pipelines                                   tekton-pipelines-webhook-f87bdc5c7-4djpt                     1/1     Running     0                 6h
</pre>


## ⛹️‍♂️ Lab - Deploying an application from GitHub Repo that has just Dockerfile and the application jar
```
oc new-app https://github.com/tektutor/openshift-spring.git
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-app https://github.com/tektutor/openshift-spring.git</b>
--> Found container image 1ffbb31 (3 days old) from docker.io for "docker.io/openjdk:latest"

    * An image stream tag will be created as "openjdk:latest" that will track the source image
    * A Docker build using source code from https://github.com/tektutor/openshift-spring.git will be created
      * The resulting image will be pushed to image stream tag "openshift-spring:latest"
      * Every time "openjdk:latest" changes a new build will be triggered

--> Creating resources ...
    imagestream.image.openshift.io "openjdk" created
    imagestream.image.openshift.io "openshift-spring" created
    buildconfig.build.openshift.io "openshift-spring" created
    deployment.apps "openshift-spring" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/openshift-spring' to track its progress.
    Run 'oc status' to view your app.
</pre>

Checking the application deployment status
```
oc status
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc status</b>
In project jegan on server https://api.tektutor.tektutor.org:6443

deployment/openshift-spring deploys istag/openshift-spring:latest <-
  bc/openshift-spring docker builds https://github.com/tektutor/openshift-spring.git on istag/openjdk:latest 
    build #1 running for 6 seconds
  deployment #1 running for 8 seconds - 0/1 pods growing to 1


1 info identified, use 'oc status --suggest' to see details.
</pre>


List the deployment, replicaset and pods
```
oc get deploy,rs,po
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get deploy,rs,po</b>
NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/openshift-spring   0/1     0            0           39s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/openshift-spring-547974b956   1         0         0       39s

NAME                           READY   STATUS    RESTARTS   AGE
pod/openshift-spring-1-build   1/1     Running   0          37s
</pre>

Checking the build logs
```
oc logs pod/openshift-spring-1-build
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc logs pod/openshift-spring-1-build</b>
time="2022-03-23T06:52:58Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I0323 06:52:58.117764       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
Caching blobs under "/var/cache/blobs".

Pulling image docker.io/openjdk@sha256:ace059a5b9e58f0fe2b6bedbb14b3a7736d17ab7b2824f3990631e47d90e6849 ...
Trying to pull docker.io/library/openjdk@sha256:ace059a5b9e58f0fe2b6bedbb14b3a7736d17ab7b2824f3990631e47d90e6849...
Getting image source signatures
Copying blob sha256:7db19695be7be96a8d27e754c74310c6fafa295ecdccbc10e8c5fe5b23854c2a
Copying blob sha256:fc6e4d76e00b859a131e9b4131d92efca34af2da17d3bdc6d5323d473c1972eb
Copying blob sha256:1824cb7e97fbcfc5c6ffea667f8e19cee765d015fef1b8ad70f96252d9713c95
Copying config sha256:1ffbb31e1412b475e57d8e2168700f534f7f54705c5212246a49ae32fe723e4a
Writing manifest to image destination
Storing signatures
Adding transient rw bind mount for /run/secrets/rhsm
STEP 1/5: FROM docker.io/openjdk@sha256:ace059a5b9e58f0fe2b6bedbb14b3a7736d17ab7b2824f3990631e47d90e6849
STEP 2/5: COPY hello.jar /app.jar
time="2022-03-23T06:53:40Z" level=warning msg="Adding metacopy option, configured globally"
--> 27f08961949
STEP 3/5: ENTRYPOINT ["java","-jar","/app.jar"]
--> 78aa8aa4228
STEP 4/5: ENV "OPENSHIFT_BUILD_NAME"="openshift-spring-1" "OPENSHIFT_BUILD_NAMESPACE"="jegan" "OPENSHIFT_BUILD_SOURCE"="https://github.com/tektutor/openshift-spring.git" "OPENSHIFT_BUILD_COMMIT"="dbb5ecb3f478141047752557f5213a4237657d88"
--> 26243d96399
STEP 5/5: LABEL "io.openshift.build.commit.author"="Jeganathan Swaminathan <mail2jegan@gmail.com>" "io.openshift.build.commit.date"="Wed Mar 23 12:19:20 2022 +0530" "io.openshift.build.commit.id"="dbb5ecb3f478141047752557f5213a4237657d88" "io.openshift.build.commit.message"="Initial commit." "io.openshift.build.commit.ref"="main" "io.openshift.build.name"="openshift-spring-1" "io.openshift.build.namespace"="jegan" "io.openshift.build.source-location"="https://github.com/tektutor/openshift-spring.git"
COMMIT temp.builder.openshift.io/jegan/openshift-spring-1:ab7a74f8
--> 3ebb1598c0f
Successfully tagged temp.builder.openshift.io/jegan/openshift-spring-1:ab7a74f8
3ebb1598c0ff2dbac2d78a8aa51d6a1b060d9ce39a6e8660cb8865f7cd8e9ddf

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/openshift-spring:latest ...
Getting image source signatures
Copying blob sha256:47956361258aabd35557f667089ad0755b2d36f0b68b8b57ba91c5f9d55519f6
Copying blob sha256:fc6e4d76e00b859a131e9b4131d92efca34af2da17d3bdc6d5323d473c1972eb
Copying blob sha256:1824cb7e97fbcfc5c6ffea667f8e19cee765d015fef1b8ad70f96252d9713c95
Copying blob sha256:7db19695be7be96a8d27e754c74310c6fafa295ecdccbc10e8c5fe5b23854c2a
Copying config sha256:3ebb1598c0ff2dbac2d78a8aa51d6a1b060d9ce39a6e8660cb8865f7cd8e9ddf
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/openshift-spring@sha256:ac74806c69a58ae3a56287ebf0640e68491517a7aa78da9d701fdcd93e4b8df0
Push successful
</pre>


Let's create a ClusterIP Internal Service for the deployment
```
oc expose deploy/openshift-spring --type=ClusterIP --port=8080
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc expose deploy/openshift-spring --type=ClusterIP --port=8080</b>
service/openshift-spring exposed
</pre>

Let's list and describe the ClusterIP service we created
```
oc get svc
oc describe svc/openshift-spring
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get svc</b>
NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
openshift-spring   ClusterIP   172.30.155.86   <none>        8080/TCP   4s
(jegan@tektutor.org)$ <b>oc describe svc/openshift-spring</b>
Name:              openshift-spring
Namespace:         jegan
Labels:            app=openshift-spring
                   app.kubernetes.io/component=openshift-spring
                   app.kubernetes.io/instance=openshift-spring
Annotations:       <none>
Selector:          deployment=openshift-spring
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.155.86
IPs:               172.30.155.86
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.2.67:8080,10.128.2.69:8080,10.128.2.70:8080
Session Affinity:  None
Events:            <none>
</pre>

Let's create a public route to test the ClusterIP Service
```
oc expose svc/openshift-spring
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc expose svc/openshift-spring</b>
route.route.openshift.io/openshift-spring exposed
</pre>

List the route
```
oc get route
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get route</b>
NAME               HOST/PORT                                           PATH   SERVICES           PORT   TERMINATION   WILDCARD
openshift-spring   openshift-spring-jegan.apps.tektutor.tektutor.org          openshift-spring   8080                 None
</pre>

Now you can try to access the public route url
```
curl openshift-spring-jegan.apps.tektutor.tektutor.org
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>curl openshift-spring-jegan.apps.tektutor.tektutor.org</b>
Greetings from Spring Boot!
</pre>

## ⛹️‍♀️ Lab - Deploying an application overriding the deployment strategy
The git repo used below also has a Dockerfile. 
By default, Openshift would have picked the Dockerfile if we haven't used  <br>--strategy=source

```
oc new-app java:openjdk-11-el7~https://github.com/tektutor/spring-ms.git --strategy=source
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-app java:openjdk-11-el7~https://github.com/tektutor/spring-ms.git --strategy=source</b>
--> Found image 385acdc (2 months old) in image stream "openshift/java" under tag "openjdk-11-el7" for "java:openjdk-11-el7"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * A source build using source code from https://github.com/tektutor/spring-ms.git will be created
      * The resulting image will be pushed to image stream tag "spring-ms:latest"
      * Use 'oc start-build' to trigger a new build

--> Creating resources ...
    imagestream.image.openshift.io "spring-ms" created
    buildconfig.build.openshift.io "spring-ms" created
    deployment.apps "spring-ms" created
    service "spring-ms" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/spring-ms' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/spring-ms' 
    Run 'oc status' to view your app.
</pre>

#### Tracking the deployment progress
```
oc logs -f buildconfig/spring-ms
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc logs -f bc/spring-ms</b>
Cloning "https://github.com/tektutor/spring-ms.git" ...
	Commit:	dd9d75b76029ced3481833315d937cfc6cf3975a (Update pom.xml)
	Author:	Jeganathan Swaminathan <jegan@tektutor.org>
	Date:	Tue Mar 1 12:51:21 2022 +0530
time="2022-03-23T03:09:50Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I0323 03:09:50.177009       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
Caching blobs under "/var/cache/blobs".
Trying to pull image-registry.openshift-image-registry.svc:5000/openshift/java@sha256:0618d4d6ebc7f40df445063c167d101d12e4955bc60b15713af43bd188104ab5...
Getting image source signatures
Copying blob sha256:e2cbb7720081a428878d2e437d9e8fb6db8683b662e41096b8337117f87e6e86
Copying blob sha256:6a903172bc76e121453f7baf4e5f0ca4127479953db19ecd571f7ce6054057b8
Copying blob sha256:6a9adb09cd70b25c97460505b84a95a49cf872f814387c67a714738ae4ace009
Copying config sha256:385acdc8d3bced53c3fb8f38020d0843615b2ab20aa71edaf0dafc995b624b48
Writing manifest to image destination
Storing signatures
Generating dockerfile with builder image image-registry.openshift-image-registry.svc:5000/openshift/java@sha256:0618d4d6ebc7f40df445063c167d101d12e4955bc60b15713af43bd188104ab5
Adding transient rw bind mount for /run/secrets/rhsm
STEP 1/9: FROM image-registry.openshift-image-registry.svc:5000/openshift/java@sha256:0618d4d6ebc7f40df445063c167d101d12e4955bc60b15713af43bd188104ab5
STEP 2/9: LABEL "io.openshift.build.commit.date"="Tue Mar 1 12:51:21 2022 +0530"       "io.openshift.build.commit.id"="dd9d75b76029ced3481833315d937cfc6cf3975a"       "io.openshift.build.commit.ref"="master"       "io.openshift.build.commit.message"="Update pom.xml"       "io.openshift.build.source-location"="https://github.com/tektutor/spring-ms.git"       "io.openshift.s2i.destination"="/tmp"       "io.openshift.build.image"="image-registry.openshift-image-registry.svc:5000/openshift/java@sha256:0618d4d6ebc7f40df445063c167d101d12e4955bc60b15713af43bd188104ab5"       "io.openshift.build.commit.author"="Jeganathan Swaminathan <jegan@tektutor.org>"
STEP 3/9: ENV OPENSHIFT_BUILD_NAME="spring-ms-1"     OPENSHIFT_BUILD_NAMESPACE="default"     OPENSHIFT_BUILD_SOURCE="https://github.com/tektutor/spring-ms.git"     OPENSHIFT_BUILD_COMMIT="dd9d75b76029ced3481833315d937cfc6cf3975a"
STEP 4/9: USER root
STEP 5/9: COPY upload/src /tmp/src
STEP 6/9: RUN chown -R 185:0 /tmp/src
STEP 7/9: USER 185
STEP 8/9: RUN /usr/local/s2i/assemble
INFO Performing Maven build in /tmp/src
INFO Using MAVEN_OPTS -XX:+UseParallelGC -XX:MinHeapFreeRatio=10 -XX:MaxHeapFreeRatio=20 -XX:GCTimeRatio=4 -XX:AdaptiveSizePolicyWeight=90 -XX:+ExitOnOutOfMemoryError
INFO Using Apache Maven 3.6.1 (Red Hat 3.6.1-6.3)
Maven home: /opt/rh/rh-maven36/root/usr/share/maven
Java version: 11.0.14, vendor: Red Hat, Inc., runtime: /usr/lib/jvm/java-11-openjdk-11.0.14.0.9-1.el7_9.x86_64
Default locale: en_US, platform encoding: ANSI_X3.4-1968
OS name: "linux", version: "4.18.0-305.34.2.el8_4.x86_64", arch: "amd64", family: "unix"
INFO Running 'mvn -e -Popenshift -DskipTests -Dcom.redhat.xpaas.repo.redhatga -Dfabric8.skip=true -Djkube.skip=true --batch-mode -Djava.net.preferIPv4Stack=true -s /tmp/artifacts/configuration/settings.xml -Dmaven.repo.local=/tmp/artifacts/m2  package'
[INFO] Error stacktraces are turned on.
[INFO] Scanning for projects...
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-parent/2.4.2/spring-boot-starter-parent-2.4.2.pom (8.6 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/2.4.2/spring-boot-dependencies-2.4.2.pom (108 kB at 155 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/datastax/oss/java-driver-bom/4.9.0/java-driver-bom-4.9.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/datastax/oss/java-driver-bom/4.9.0/java-driver-bom-4.9.0.pom (4.1 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-bom/4.1.17/metrics-bom-4.1.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-bom/4.1.17/metrics-bom-4.1.17.pom (5.3 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-parent/4.1.17/metrics-parent-4.1.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/dropwizard/metrics/metrics-parent/4.1.17/metrics-parent-4.1.17.pom (17 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/groovy/groovy-bom/2.5.14/groovy-bom-2.5.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/groovy/groovy-bom/2.5.14/groovy-bom-2.5.14.pom (26 kB at 59 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-bom/11.0.8.Final/infinispan-bom-11.0.8.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-bom/11.0.8.Final/infinispan-bom-11.0.8.Final.pom (19 kB at 48 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-build-configuration-parent/11.0.8.Final/infinispan-build-configuration-parent-11.0.8.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/infinispan/infinispan-build-configuration-parent/11.0.8.Final/infinispan-build-configuration-parent-11.0.8.Final.pom (13 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/jboss-parent/36/jboss-parent-36.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/jboss-parent/36/jboss-parent-36.pom (66 kB at 156 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-bom/2.11.4/jackson-bom-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-bom/2.11.4/jackson-bom-2.11.4.pom (15 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-parent/2.11/jackson-parent-2.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-parent/2.11/jackson-parent-2.11.pom (7.8 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/oss-parent/38/oss-parent-38.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/oss-parent/38/oss-parent-38.pom (23 kB at 58 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jersey/jersey-bom/2.32/jersey-bom-2.32.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jersey/jersey-bom/2.32/jersey-bom-2.32.pom (19 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.6/project-1.0.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.6/project-1.0.6.pom (13 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-bom/9.4.35.v20201120/jetty-bom-9.4.35.v20201120.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/jetty/jetty-bom/9.4.35.v20201120/jetty-bom-9.4.35.v20201120.pom (18 kB at 42 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.7.0/junit-bom-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.7.0/junit-bom-5.7.0.pom (5.1 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-bom/1.4.21/kotlin-bom-1.4.21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jetbrains/kotlin/kotlin-bom/1.4.21/kotlin-bom-1.4.21.pom (9.3 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jetbrains/kotlinx/kotlinx-coroutines-bom/1.4.2/kotlinx-coroutines-bom-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jetbrains/kotlinx/kotlinx-coroutines-bom/1.4.2/kotlinx-coroutines-bom-1.4.2.pom (4.1 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-bom/2.13.3/log4j-bom-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-bom/2.13.3/log4j-bom-2.13.3.pom (7.6 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/logging-parent/1/logging-parent-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/logging-parent/1/logging-parent-1.pom (3.2 kB at 7.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/18/apache-18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/18/apache-18.pom (16 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/micrometer/micrometer-bom/1.6.3/micrometer-bom-1.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/micrometer/micrometer-bom/1.6.3/micrometer-bom-1.6.3.pom (6.8 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/netty/netty-bom/4.1.58.Final/netty-bom-4.1.58.Final.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/netty/netty-bom/4.1.58.Final/netty-bom-4.1.58.Final.pom (8.8 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/7/oss-parent-7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/7/oss-parent-7.pom (4.8 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc-bom/19.8.0.0/ojdbc-bom-19.8.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/oracle/database/jdbc/ojdbc-bom/19.8.0.0/ojdbc-bom-19.8.0.0.pom (12 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/r2dbc/r2dbc-bom/Arabba-SR8/r2dbc-bom-Arabba-SR8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/r2dbc/r2dbc-bom/Arabba-SR8/r2dbc-bom-Arabba-SR8.pom (4.1 kB at 9.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/projectreactor/reactor-bom/2020.0.3/reactor-bom-2020.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/projectreactor/reactor-bom/2020.0.3/reactor-bom-2020.0.3.pom (4.5 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/io/rsocket/rsocket-bom/1.1.0/rsocket-bom-1.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/io/rsocket/rsocket-bom/1.1.0/rsocket-bom-1.1.0.pom (2.6 kB at 6.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/data/spring-data-bom/2020.0.3/spring-data-bom-2020.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/data/spring-data-bom/2020.0.3/spring-data-bom-2020.0.3.pom (5.9 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/5.3.3/spring-framework-bom-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/5.3.3/spring-framework-bom-5.3.3.pom (5.6 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/integration/spring-integration-bom/5.4.3/spring-integration-bom-5.4.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/integration/spring-integration-bom/5.4.3/spring-integration-bom-5.4.3.pom (9.5 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/security/spring-security-bom/5.4.2/spring-security-bom-5.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/security/spring-security-bom/5.4.2/spring-security-bom-5.4.2.pom (5.3 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/session/spring-session-bom/2020.0.1/spring-session-bom-2020.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/session/spring-session-bom/2020.0.1/spring-session-bom-2020.0.1.pom (2.7 kB at 5.7 kB/s)
[INFO] 
[INFO] ---------------------< org.tektutor:spring-hello >----------------------
[INFO] Building spring-boot 1.0
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.pom (2.9 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-maven-plugin/2.4.2/spring-boot-maven-plugin-2.4.2.jar (101 kB at 223 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.pom (8.1 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/34/maven-plugins-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/34/maven-plugins-34.pom (11 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/34/maven-parent-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/34/maven-parent-34.pom (43 kB at 108 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/23/apache-23.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/23/apache-23.pom (18 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-resources-plugin/3.2.0/maven-resources-plugin-3.2.0.jar (33 kB at 67 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.pom (12 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/33/maven-plugins-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-plugins/33/maven-plugins-33.pom (11 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/33/maven-parent-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/33/maven-parent-33.pom (44 kB at 112 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/21/apache-21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/21/apache-21.pom (17 kB at 45 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/3.8.1/maven-compiler-plugin-3.8.1.jar (62 kB at 152 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.pom (5.0 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire/2.22.2/surefire-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire/2.22.2/surefire-2.22.2.pom (26 kB at 67 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-surefire-plugin/2.22.2/maven-surefire-plugin-2.22.2.jar (41 kB at 103 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.pom (7.3 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugins/maven-jar-plugin/3.2.0/maven-jar-plugin-3.2.0.jar (29 kB at 68 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.pom (3.0 kB at 7.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.pom (3.1 kB at 8.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.pom (2.2 kB at 5.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.pom (2.0 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.pom (1.8 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.pom (2.6 kB at 6.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.pom (2.2 kB at 5.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.pom (2.0 kB at 5.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.pom (2.1 kB at 5.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.pom (2.1 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.pom (2.5 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.pom (13 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-parent/1.2.3/logback-parent-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-parent/1.2.3/logback-parent-1.2.3.pom (18 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.pom (4.2 kB at 9.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.pom (3.8 kB at 9.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.7.30/slf4j-parent-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.7.30/slf4j-parent-1.7.30.pom (14 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.pom (7.2 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j/2.13.3/log4j-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j/2.13.3/log4j-2.13.3.pom (64 kB at 127 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.pom (13 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.pom (990 B at 2.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.pom (16 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/ca-parent/1.3.5/ca-parent-1.3.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/ca-parent/1.3.5/ca-parent-1.3.5.pom (2.8 kB at 7.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.5/project-1.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/ee4j/project/1.0.5/project-1.0.5.pom (13 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.pom (37 kB at 93 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.pom (3.1 kB at 6.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.pom (2.2 kB at 5.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.pom (7.4 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-base/2.11.4/jackson-base-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-base/2.11.4/jackson-base-2.11.4.pom (7.5 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.pom (4.6 kB at 9.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.pom (4.9 kB at 9.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.pom (2.2 kB at 5.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-modules-java8/2.11.4/jackson-modules-java8-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-modules-java8/2.11.4/jackson-modules-java8-2.11.4.pom (3.2 kB at 8.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.pom (4.5 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.pom (4.0 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.pom (3.1 kB at 7.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.pom (1.8 kB at 4.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.pom (14 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.pom (1.8 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.pom (2.9 kB at 7.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.pom (4.7 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.pom (2.1 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.pom (2.5 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.pom (2.6 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.pom (9.0 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/minidev-parent/2.3/minidev-parent-2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/minidev-parent/2.3/minidev-parent-2.3.pom (8.5 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.pom (12 kB at 29 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.pom (1.9 kB at 3.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm-parent/5.0.4/asm-parent-5.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm-parent/5.0.4/asm-parent-5.0.4.pom (5.5 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.3/ow2-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.3/ow2-1.3.pom (9.5 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.pom (13 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api-parent/2.3.3/jakarta.xml.bind-api-parent-2.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api-parent/2.3.3/jakarta.xml.bind-api-parent-2.3.3.pom (9.0 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.pom (5.3 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/sun/activation/all/1.2.2/all-1.2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/sun/activation/all/1.2.2/all-1.2.2.pom (15 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.pom (24 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-parent-pom/2.2.8/assertj-parent-pom-2.2.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-parent-pom/2.2.8/assertj-parent-pom-2.2.8.pom (24 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.6.3/junit-bom-5.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/junit-bom/5.6.3/junit-bom-5.6.3.pom (4.9 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.pom (1.1 kB at 2.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.pom (3.2 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.pom (3.2 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.pom (1.2 kB at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.pom (1.7 kB at 4.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.pom (2.8 kB at 7.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.pom (3.0 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.pom (3.2 kB at 8.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.pom (3.2 kB at 7.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.pom (2.9 kB at 5.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.pom (11 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-parent/1.10.19/byte-buddy-parent-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-parent/1.10.19/byte-buddy-parent-1.10.19.pom (42 kB at 83 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.pom (9.6 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.pom (3.5 kB at 7.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis-parent/3.1/objenesis-parent-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis-parent/3.1/objenesis-parent-3.1.pom (18 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.pom (2.7 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.pom (5.2 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.pom (2.8 kB at 6.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.pom (2.0 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.pom (2.7 kB at 7.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-parent/2.7.0/xmlunit-parent-2.7.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-parent/2.7.0/xmlunit-parent-2.7.0.pom (20 kB at 49 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter/2.4.2/spring-boot-starter-2.4.2.jar (4.8 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-logging/2.4.2/spring-boot-starter-logging-2.4.2.jar (4.8 kB at 5.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-web/2.4.2/spring-boot-starter-web-2.4.2.jar (4.8 kB at 5.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-classic/1.2.3/logback-classic-1.2.3.jar (290 kB at 227 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-to-slf4j/2.13.3/log4j-to-slf4j-2.13.3.jar (17 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jul-to-slf4j/1.7.30/jul-to-slf4j-1.7.30.jar (4.6 kB at 2.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/ch/qos/logback/logback-core/1.2.3/logback-core-1.2.3.jar (472 kB at 244 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/logging/log4j/log4j-api/2.13.3/log4j-api-2.13.3.jar (292 kB at 150 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/annotation/jakarta.annotation-api/1.3.5/jakarta.annotation-api-1.3.5.jar (25 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-json/2.4.2/spring-boot-starter-json-2.4.2.jar (4.7 kB at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/yaml/snakeyaml/1.27/snakeyaml-1.27.jar (310 kB at 130 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-annotations/2.11.4/jackson-annotations-2.11.4.jar (72 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-core/2.11.4/jackson-core-2.11.4.jar (352 kB at 124 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jdk8/2.11.4/jackson-datatype-jdk8-2.11.4.jar (34 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/datatype/jackson-datatype-jsr310/2.11.4/jackson-datatype-jsr310-2.11.4.jar (111 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.11.4/jackson-databind-2.11.4.jar (1.4 MB at 423 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/fasterxml/jackson/module/jackson-module-parameter-names/2.11.4/jackson-module-parameter-names-2.11.4.jar (9.3 kB at 2.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-tomcat/2.4.2/spring-boot-starter-tomcat-2.4.2.jar (4.8 kB at 1.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/glassfish/jakarta.el/3.0.3/jakarta.el-3.0.3.jar (238 kB at 60 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-websocket/9.0.41/tomcat-embed-websocket-9.0.41.jar (272 kB at 67 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-web/5.3.3/spring-web-5.3.3.jar (1.6 MB at 349 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-beans/5.3.3/spring-beans-5.3.3.jar (696 kB at 149 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-webmvc/5.3.3/spring-webmvc-5.3.3.jar (996 kB at 202 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-aop/5.3.3/spring-aop-5.3.3.jar (374 kB at 74 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-context/5.3.3/spring-context-5.3.3.jar (1.2 MB at 230 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-expression/5.3.3/spring-expression-5.3.3.jar (283 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot/2.4.2/spring-boot-2.4.2.jar (1.3 MB at 234 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-starter-test/2.4.2/spring-boot-starter-test-2.4.2.jar (4.8 kB at 826 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test/2.4.2/spring-boot-test-2.4.2.jar (219 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/jayway/jsonpath/json-path/2.4.0/json-path-2.4.0.jar (223 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/json-smart/2.3/json-smart-2.3.jar (120 kB at 19 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-test-autoconfigure/2.4.2/spring-boot-test-autoconfigure-2.4.2.jar (182 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/minidev/accessors-smart/1.2/accessors-smart-1.2.jar (30 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/tomcat/embed/tomcat-embed-core/9.0.41/tomcat-embed-core-9.0.41.jar (3.4 MB at 516 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/5.0.4/asm-5.0.4.jar (53 kB at 8.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/xml/bind/jakarta.xml.bind-api/2.3.3/jakarta.xml.bind-api-2.3.3.jar (116 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.7.30/slf4j-api-1.7.30.jar (41 kB at 5.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/jakarta/activation/jakarta.activation-api/1.2.2/jakarta.activation-api-1.2.2.jar (47 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest/2.2/hamcrest-2.2.jar (123 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter/5.7.0/junit-jupiter-5.7.0.jar (6.4 kB at 856 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/assertj/assertj-core/3.18.1/assertj-core-3.18.1.jar (4.8 MB at 641 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-api/5.7.0/junit-jupiter-api-5.7.0.jar (175 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apiguardian/apiguardian-api/1.1.0/apiguardian-api-1.1.0.jar (2.4 kB at 307 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/opentest4j/opentest4j/1.2.0/opentest4j-1.2.0.jar (7.7 kB at 979 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-commons/1.7.0/junit-platform-commons-1.7.0.jar (100 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-autoconfigure/2.4.2/spring-boot-autoconfigure-2.4.2.jar (1.5 MB at 195 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-engine/5.7.0/junit-jupiter-engine-5.7.0.jar (212 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-core/3.6.28/mockito-core-3.6.28.jar (675 kB at 81 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/jupiter/junit-jupiter-params/5.7.0/junit-jupiter-params-5.7.0.jar (567 kB at 68 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy-agent/1.10.19/byte-buddy-agent-1.10.19.jar (259 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/mockito/mockito-junit-jupiter/3.6.28/mockito-junit-jupiter-3.6.28.jar (4.9 kB at 566 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/objenesis/objenesis/3.1/objenesis-3.1.jar (60 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/junit/platform/junit-platform-engine/1.7.0/junit-platform-engine-1.7.0.jar (181 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/skyscreamer/jsonassert/1.5.0/jsonassert-1.5.0.jar (30 kB at 3.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/vaadin/external/google/android-json/0.0.20131108.vaadin1/android-json-0.0.20131108.vaadin1.jar (18 kB at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-jcl/5.3.3/spring-jcl-5.3.3.jar (24 kB at 2.6 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-test/5.3.3/spring-test-5.3.3.jar (763 kB at 82 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/xmlunit/xmlunit-core/2.7.0/xmlunit-core-2.7.0.jar (168 kB at 18 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/spring-core/5.3.3/spring-core-5.3.3.jar (1.5 MB at 145 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/bytebuddy/byte-buddy/1.10.19/byte-buddy-1.10.19.jar (3.5 MB at 278 kB/s)
[INFO] 
[INFO] --- maven-resources-plugin:3.2.0:resources (default-resources) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.pom (3.0 kB at 6.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.1.0/maven-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.1.0/maven-3.1.0.pom (22 kB at 42 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/23/maven-parent-23.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/23/maven-parent-23.pom (33 kB at 65 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/13/apache-13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/13/apache-13.pom (14 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.pom (3.8 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.pom (3.1 kB at 6.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3/plexus-3.3.pom (20 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/17/spice-parent-17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/17/spice-parent-17.pom (6.8 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/10/forge-parent-10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/10/forge-parent-10.pom (14 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.pom (1.6 kB at 4.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.pom (5.9 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.0.0.M2a/sisu-plexus-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.0.0.M2a/sisu-plexus-0.0.0.M2a.pom (7.9 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.pom (1.4 kB at 2.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-parent/1.0/weld-api-parent-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-parent/1.0/weld-api-parent-1.0.pom (2.4 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-bom/1.0/weld-api-bom-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-api-bom/1.0/weld-api-bom-1.0.pom (7.9 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-parent/6/weld-parent-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/jboss/weld/weld-parent/6/weld-parent-6.pom (21 kB at 41 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.pom (1.0 kB at 2.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.pom (612 B at 1.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.pom (5.4 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava-parent/10.0.1/guava-parent-10.0.1.pom (2.0 kB at 5.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.pom (965 B at 2.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0.pom (10 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-parent/3.1.0/guice-parent-3.1.0.pom (11 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.pom (363 B at 825 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.pom (5.0 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.0.0.M2a/sisu-inject-0.0.0.M2a.pom (7.8 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.pom (266 B at 536 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm-parent/3.3.1/asm-parent-3.3.1.pom (4.3 kB at 8.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.pom (750 B at 1.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/2.0.0/plexus-containers-2.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/2.0.0/plexus-containers-2.0.0.pom (4.8 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/5.1/plexus-5.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/5.1/plexus-5.1.pom (23 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4/plexus-classworlds-2.4.pom (3.9 kB at 7.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.7/plexus-2.0.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.7/plexus-2.0.7.pom (17 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.1/plexus-utils-2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.1/plexus-utils-2.1.pom (4.0 kB at 9.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/16/spice-parent-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/16/spice-parent-16.pom (8.4 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/5/forge-parent-5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/5/forge-parent-5.pom (8.4 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.pom (6.9 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.pom (1.8 kB at 3.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.pom (2.3 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.16/plexus-interpolation-1.16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.16/plexus-interpolation-1.16.pom (1.0 kB at 2.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.3/plexus-components-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.3/plexus-components-1.3.pom (3.1 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.pom (3.0 kB at 5.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/12/spice-parent-12.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/12/spice-parent-12.pom (6.8 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/4/forge-parent-4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/4/forge-parent-4.pom (8.4 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.5/plexus-utils-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.5/plexus-utils-1.5.5.pom (5.1 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.11/plexus-1.0.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.11/plexus-1.0.11.pom (9.0 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.pom (2.1 kB at 4.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.pom (1.9 kB at 3.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.pom (2.5 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.pom (3.5 kB at 7.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.pom (1.7 kB at 3.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether/0.9.0.M2/aether-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether/0.9.0.M2/aether-0.9.0.M2.pom (28 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.pom (1.8 kB at 3.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.pom (2.0 kB at 4.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.pom (3.3 kB at 6.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.pom (3.5 kB at 8.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.0.1/plexus-3.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.0.1/plexus-3.0.1.pom (19 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.pom (2.7 kB at 5.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.pom (6.8 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/34/maven-shared-components-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/34/maven-shared-components-34.pom (5.1 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.pom (5.8 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.pom (14 kB at 28 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/42/commons-parent-42.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/42/commons-parent-42.pom (68 kB at 135 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.pom (5.2 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.pom (3.2 kB at 6.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/15/spice-parent-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/spice/spice-parent/15/spice-parent-15.pom (8.4 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.8/plexus-utils-1.5.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.8/plexus-utils-1.5.8.pom (8.1 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.2/plexus-2.0.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.2/plexus-2.0.2.pom (12 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.pom (28 kB at 56 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/47/commons-parent-47.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/47/commons-parent-47.pom (78 kB at 176 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/19/apache-19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/19/apache-19.pom (15 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.1.0/maven-artifact-3.1.0.jar (52 kB at 128 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.1.0/maven-settings-3.1.0.jar (47 kB at 111 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.1.0/maven-core-3.1.0.jar (563 kB at 1.3 MB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.1.0/maven-plugin-api-3.1.0.jar (50 kB at 115 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.1.0/maven-settings-builder-3.1.0.jar (41 kB at 85 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.1.0/maven-repository-metadata-3.1.0.jar (30 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.1.0/maven-model-builder-3.1.0.jar (159 kB at 154 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-api/0.9.0.M2/aether-api-0.9.0.M2.jar (134 kB at 109 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-impl/0.9.0.M2/aether-impl-0.9.0.M2.jar (145 kB at 115 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-util/0.9.0.M2/aether-util-0.9.0.M2.jar (134 kB at 87 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.10/plexus-utils-3.0.10.jar (231 kB at 142 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.4.2/plexus-classworlds-2.4.2.jar (47 kB at 27 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/aether/aether-spi/0.9.0.M2/aether-spi-0.9.0.M2.jar (18 kB at 9.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-sec-dispatcher/1.3/plexus-sec-dispatcher-1.3.jar (29 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-cipher/1.4/plexus-cipher-1.4.jar (13 kB at 6.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/2.0.0/plexus-component-annotations-2.0.0.jar (4.2 kB at 1.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.26/plexus-interpolation-1.26.jar (85 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.0.0.M2a/org.eclipse.sisu.plexus-0.0.0.M2a.jar (202 kB at 84 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.1.0/maven-aether-provider-3.1.0.jar (60 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.1.0/maven-model-3.1.0.jar (164 kB at 65 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/enterprise/cdi-api/1.0/cdi-api-1.0.jar (45 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0-no_aop.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/annotation/jsr250-api/1.0/jsr250-api-1.0.jar (5.8 kB at 2.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/guava/guava/10.0.1/guava-10.0.1.jar (1.5 MB at 529 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/javax/inject/javax.inject/1/javax.inject-1.jar (2.5 kB at 866 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/1.3.9/jsr305-1.3.9.jar (33 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/aopalliance/aopalliance/1.0/aopalliance-1.0.jar (4.5 kB at 1.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.0.0.M2a/org.eclipse.sisu.inject-0.0.0.M2a.jar (202 kB at 62 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/3.1.0/sisu-guice-3.1.0-no_aop.jar (357 kB at 109 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-filtering/3.2.0/maven-filtering-3.2.0.jar (52 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/plexus/plexus-build-api/0.0.7/plexus-build-api-0.0.7.jar (8.5 kB at 2.3 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.3.3/maven-shared-utils-3.3.3.jar (154 kB at 42 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.6/commons-io-2.6.jar (215 kB at 56 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/asm/asm/3.3.1/asm-3.3.1.jar (44 kB at 9.9 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-lang3/3.8.1/commons-lang3-3.8.1.jar (502 kB at 102 kB/s)
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] skip non existing resourceDirectory /tmp/src/src/main/resources
[INFO] skip non existing resourceDirectory /tmp/src/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:compile (default-compile) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.pom (2.3 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.0/maven-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.0/maven-3.0.pom (22 kB at 51 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/15/maven-parent-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/15/maven-parent-15.pom (24 kB at 52 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/6/apache-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/6/apache-6.pom (13 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.pom (3.9 kB at 9.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.pom (3.3 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.6/plexus-2.0.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.6/plexus-2.0.6.pom (17 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.pom (1.9 kB at 3.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.pom (5.4 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-plexus/1.4.2/guice-plexus-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-plexus/1.4.2/guice-plexus-1.4.2.pom (3.1 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-bean/1.4.2/guice-bean-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/inject/guice-bean/1.4.2/guice-bean-1.4.2.pom (2.6 kB at 6.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject/1.4.2/sisu-inject-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject/1.4.2/sisu-inject-1.4.2.pom (1.2 kB at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-parent/1.4.2/sisu-parent-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-parent/1.4.2/sisu-parent-1.4.2.pom (7.8 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/6/forge-parent-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/forge/forge-parent/6/forge-parent-6.pom (11 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.pom (770 B at 1.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.7.1/plexus-containers-1.7.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.7.1/plexus-containers-1.7.1.pom (5.0 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/4.0/plexus-4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/4.0/plexus-4.0.pom (22 kB at 32 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.pom (4.0 kB at 9.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.5/plexus-utils-2.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.5/plexus-utils-2.0.5.pom (3.3 kB at 6.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.pom (5.5 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7.pom (11 kB at 22 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.pom (6.6 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.pom (1.9 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.pom (2.2 kB at 4.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.pom (910 B at 1.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.18/plexus-components-1.1.18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.18/plexus-components-1.1.18.pom (5.4 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.pom (1.9 kB at 3.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.pom (2.2 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.pom (2.5 kB at 6.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.pom (1.7 kB at 4.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-parent/1.7/aether-parent-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-parent/1.7/aether-parent-1.7.pom (7.7 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.pom (2.1 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.pom (3.7 kB at 7.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.pom (1.7 kB at 4.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.pom (5.6 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/30/maven-shared-components-30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/30/maven-shared-components-30.pom (4.6 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/30/maven-parent-30.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/30/maven-parent-30.pom (41 kB at 82 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.pom (13 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/39/commons-parent-39.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/39/commons-parent-39.pom (62 kB at 132 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/16/apache-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/16/apache-16.pom (15 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.pom (4.7 kB at 9.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/19/maven-shared-components-19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/19/maven-shared-components-19.pom (6.4 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.pom (1.5 kB at 3.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/2.2.1/maven-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/2.2.1/maven-2.2.1.pom (22 kB at 44 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/11/maven-parent-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/11/maven-parent-11.pom (32 kB at 65 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/5/apache-5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/5/apache-5.pom (4.1 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.pom (12 kB at 23 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.pom (2.2 kB at 5.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.pom (3.2 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.pom (6.8 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.pom (889 B at 1.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.14/plexus-components-1.1.14.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/1.1.14/plexus-components-1.1.14.pom (5.8 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.pom (2.0 kB at 3.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.pom (1.9 kB at 3.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.5.6/slf4j-parent-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-parent/1.5.6/slf4j-parent-1.5.6.pom (7.9 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.pom (3.0 kB at 7.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.pom (2.2 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.pom (2.2 kB at 4.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.pom (1.6 kB at 3.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.pom (1.9 kB at 3.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.pom (1.7 kB at 3.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.pom (2.8 kB at 7.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.pom (3.1 kB at 8.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.pom (880 B at 2.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.pom (1.9 kB at 5.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.pom (2.1 kB at 4.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.pom (1.3 kB at 2.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/0.1/maven-shared-utils-0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/0.1/maven-shared-utils-0.1.pom (4.0 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/18/maven-shared-components-18.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/18/maven-shared-components-18.pom (4.9 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/22/maven-parent-22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/22/maven-parent-22.pom (30 kB at 59 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/11/apache-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/11/apache-11.pom (15 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/2.0.1/jsr305-2.0.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/google/code/findbugs/jsr305/2.0.1/jsr305-2.0.1.pom (965 B at 1.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.pom (5.1 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-languages/0.9.10/plexus-languages-0.9.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-languages/0.9.10/plexus-languages-0.9.10.pom (4.1 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.pom (2.9 kB at 7.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.5/ow2-1.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/ow2/1.5/ow2-1.5.pom (11 kB at 29 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.pom (16 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/9/oss-parent-9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/oss/oss-parent/9/oss-parent-9.pom (6.6 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.pom (867 B at 1.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler/2.8.4/plexus-compiler-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler/2.8.4/plexus-compiler-2.8.4.pom (6.0 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/4.0/plexus-components-4.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-components/4.0/plexus-components-4.0.pom (2.7 kB at 1.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.22/plexus-utils-3.0.22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.22/plexus-utils-3.0.22.pom (3.8 kB at 7.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3.1/plexus-3.3.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/3.3.1/plexus-3.3.1.pom (20 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.pom (692 B at 1.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.pom (771 B at 2.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compilers/2.8.4/plexus-compilers-2.8.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compilers/2.8.4/plexus-compilers-2.8.4.pom (1.3 kB at 3.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7-noaop.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.0/maven-model-3.0.jar (165 kB at 98 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-bean/1.4.2/sisu-inject-bean-1.4.2.jar (153 kB at 89 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.0/maven-plugin-api-3.0.jar (49 kB at 27 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.0/maven-artifact-3.0.jar (52 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/2.0.4/plexus-utils-2.0.4.jar (222 kB at 91 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/3.0/maven-settings-3.0.jar (47 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings-builder/3.0/maven-settings-builder-3.0.jar (38 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/3.0/maven-repository-metadata-3.0.jar (30 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-aether-provider/3.0/maven-aether-provider-3.0.jar (51 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model-builder/3.0/maven-model-builder-3.0.jar (148 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-guice/2.1.7/sisu-guice-2.1.7-noaop.jar (472 kB at 124 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-spi/1.7/aether-spi-1.7.jar (14 kB at 3.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-impl/1.7/aether-impl-1.7.jar (106 kB at 27 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-api/1.7/aether-api-1.7.jar (74 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/aether/aether-util/1.7/aether-util-1.7.jar (108 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.14/plexus-interpolation-1.14.jar (61 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.2.3/plexus-classworlds-2.2.3.jar (46 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.7.1/plexus-component-annotations-1.7.1.jar (4.3 kB at 911 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/sonatype/sisu/sisu-inject-plexus/1.4.2/sisu-inject-plexus-1.4.2.jar (202 kB at 43 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.2.1/maven-shared-utils-3.2.1.jar (167 kB at 34 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.5/commons-io-2.5.jar (209 kB at 42 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-java/0.9.10/plexus-java-0.9.10.jar (39 kB at 7.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-incremental/1.1/maven-shared-incremental-1.1.jar (14 kB at 2.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M9/qdox-2.0-M9.jar (317 kB at 59 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/ow2/asm/asm/6.2/asm-6.2.jar (111 kB at 20 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-api/2.8.4/plexus-compiler-api-2.8.4.jar (27 kB at 4.9 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-manager/2.8.4/plexus-compiler-manager-2.8.4.jar (4.7 kB at 839 B/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-compiler-javac/2.8.4/plexus-compiler-javac-2.8.4.jar (21 kB at 3.7 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/3.0/maven-core-3.0.jar (527 kB at 91 kB/s)
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 2 source files to /tmp/target
[INFO] 
[INFO] --- maven-resources-plugin:3.2.0:testResources (default-testResources) @ spring-hello ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Using 'UTF-8' encoding to copy filtered properties files.
[INFO] skip non existing resourceDirectory /tmp/src/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.1:testCompile (default-testCompile) @ spring-hello ---
[INFO] No sources to compile
[INFO] 
[INFO] --- maven-surefire-plugin:2.22.2:test (default-test) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.pom (11 kB at 24 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.pom (1.6 kB at 3.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-tools/3.5.2/maven-plugin-tools-3.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-tools/3.5.2/maven-plugin-tools-3.5.2.pom (15 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/31/maven-parent-31.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/31/maven-parent-31.pom (43 kB at 86 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.pom (3.5 kB at 7.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.pom (2.0 kB at 3.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.pom (7.5 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.pom (3.9 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.0.3/plexus-containers-1.0.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.0.3/plexus-containers-1.0.3.pom (492 B at 1.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.4/plexus-1.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/1.0.4/plexus-1.0.4.pom (5.7 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.pom (24 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.pom (766 B at 1.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-parent/1.3/hamcrest-parent-1.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-parent/1.3/hamcrest-parent-1.3.pom (2.0 kB at 1.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.0.4/plexus-utils-1.0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.0.4/plexus-utils-1.0.4.pom (6.9 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1-alpha-2/classworlds-1.1-alpha-2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1-alpha-2/classworlds-1.1-alpha-2.pom (3.1 kB at 7.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.pom (2.4 kB at 4.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/15/maven-shared-components-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/15/maven-shared-components-15.pom (9.3 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/16/maven-parent-16.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/16/maven-parent-16.pom (23 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/7/apache-7.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/7/apache-7.pom (14 kB at 29 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.pom (3.3 kB at 311 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.pom (3.3 kB at 7.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.pom (16 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/plugin-tools/maven-plugin-annotations/3.5.2/maven-plugin-annotations-3.5.2.jar (14 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-logger-api/2.22.2/surefire-logger-api-2.22.2.jar (13 kB at 27 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/2.2.1/maven-plugin-api-2.2.1.jar (12 kB at 25 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.5.15/plexus-utils-1.5.15.jar (228 kB at 256 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/2.2.1/maven-artifact-2.2.1.jar (80 kB at 84 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-booter/2.22.2/surefire-booter-2.22.2.jar (274 kB at 223 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-descriptor/2.2.1/maven-plugin-descriptor-2.2.1.jar (39 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-container-default/1.0-alpha-9-stable-1/plexus-container-default-1.0-alpha-9-stable-1.jar (194 kB at 121 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/maven-surefire-common/2.22.2/maven-surefire-common-2.22.2.jar (528 kB at 276 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar (315 kB at 157 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-project/2.2.1/maven-project-2.2.1.jar (156 kB at 73 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/hamcrest/hamcrest-core/1.3/hamcrest-core-1.3.jar (45 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-settings/2.2.1/maven-settings-2.2.1.jar (49 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-profile/2.2.1/maven-profile-2.2.1.jar (35 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact-manager/2.2.1/maven-artifact-manager-2.2.1.jar (68 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/backport-util-concurrent/backport-util-concurrent/3.1/backport-util-concurrent-3.1.jar (332 kB at 125 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/surefire/surefire-api/2.22.2/surefire-api-2.22.2.jar (186 kB at 67 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-registry/2.2.1/maven-plugin-registry-2.2.1.jar (30 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.11/plexus-interpolation-1.11.jar (51 kB at 17 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-core/2.2.1/maven-core-2.2.1.jar (178 kB at 58 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/2.2.1/maven-model-2.2.1.jar (88 kB at 29 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-jdk14/1.5.6/slf4j-jdk14-1.5.6.jar (8.8 kB at 2.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/slf4j-api/1.5.6/slf4j-api-1.5.6.jar (22 kB at 6.1 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/reporting/maven-reporting-api/3.0/maven-reporting-api-3.0.jar (11 kB at 3.0 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-parameter-documenter/2.2.1/maven-plugin-parameter-documenter-2.2.1.jar (22 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/slf4j/jcl-over-slf4j/1.5.6/jcl-over-slf4j-1.5.6.jar (17 kB at 4.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-repository-metadata/2.2.1/maven-repository-metadata-2.2.1.jar (26 kB at 6.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-error-diagnostics/2.2.1/maven-error-diagnostics-2.2.1.jar (13 kB at 3.2 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-monitor/2.2.1/maven-monitor-2.2.1.jar (10 kB at 2.6 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-toolchain/2.2.1/maven-toolchain-2.2.1.jar (38 kB at 9.1 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/classworlds/classworlds/1.1/classworlds-1.1.jar (38 kB at 8.6 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/com/thoughtworks/qdox/qdox/2.0-M8/qdox-2.0-M8.jar (316 kB at 68 kB/s)
[INFO] Tests are skipped.
[INFO] 
[INFO] --- maven-jar-plugin:3.2.0:jar (default-jar) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.pom (4.7 kB at 9.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/22/maven-shared-components-22.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/22/maven-shared-components-22.pom (5.1 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/27/maven-parent-27.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/27/maven-parent-27.pom (41 kB at 80 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/17/apache-17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/17/apache-17.pom (16 kB at 40 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.pom (4.2 kB at 8.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.pom (4.0 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/1.0-beta-6/wagon-provider-api-1.0-beta-6.pom (1.8 kB at 4.2 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/wagon-1.0-beta-6.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/1.0-beta-6/wagon-1.0-beta-6.pom (12 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/plexus-utils-1.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/1.4.2/plexus-utils-1.4.2.pom (2.0 kB at 963 B/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.pom (1.7 kB at 2.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-2.10.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon/2.10/wagon-2.10.pom (21 kB at 46 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/26/maven-parent-26.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/26/maven-parent-26.pom (40 kB at 79 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/plexus-utils-3.0.15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.15/plexus-utils-3.0.15.pom (3.1 kB at 8.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.0.0/maven-shared-utils-3.0.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.0.0/maven-shared-utils-3.0.0.pom (5.6 kB at 15 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/21/maven-shared-components-21.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/21/maven-shared-components-21.pom (5.1 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-parent/25/maven-parent-25.pom (37 kB at 72 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/15/apache-15.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/15/apache-15.pom (15 kB at 38 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.4/commons-io-2.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-io/commons-io/2.4/commons-io-2.4.pom (10 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/25/commons-parent-25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/25/commons-parent-25.pom (48 kB at 114 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/apache/9/apache-9.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/apache/9/apache-9.pom (15 kB at 31 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.pom (4.5 kB at 9.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/33/maven-shared-components-33.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-components/33/maven-shared-components-33.pom (5.1 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.0/plexus-archiver-4.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.0/plexus-archiver-4.2.0.pom (4.8 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.pom (4.5 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.pom (18 kB at 45 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/48/commons-parent-48.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/48/commons-parent-48.pom (72 kB at 143 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.pom (15 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.pom (1.9 kB at 4.6 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.25/plexus-interpolation-1.25.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-interpolation/1.25/plexus-interpolation-1.25.pom (2.6 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.pom (4.8 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/wagon/wagon-provider-api/2.10/wagon-provider-api-2.10.jar (54 kB at 77 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/file-management/3.0.0/file-management-3.0.0.jar (35 kB at 51 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-archiver/3.5.0/maven-archiver-3.5.0.jar (26 kB at 37 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-io/3.0.0/maven-shared-io-3.0.0.jar (41 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-compat/3.0/maven-compat-3.0.jar (285 kB at 242 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-io/3.2.0/plexus-io-3.2.0.jar (76 kB at 61 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-archiver/4.2.1/plexus-archiver-4.2.1.jar (196 kB at 147 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.3.0/plexus-utils-3.3.0.jar (263 kB at 161 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.19/commons-compress-1.19.jar (615 kB at 339 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/iq80/snappy/snappy/0.4/snappy-0.4.jar (58 kB at 27 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/tukaani/xz/1.8/xz-1.8.jar (109 kB at 50 kB/s)
[INFO] Building jar: /tmp/src/target/spring-hello-1.0.jar
[INFO] 
[INFO] --- spring-boot-maven-plugin:2.4.2:repackage (repackage) @ spring-hello ---
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.pom (3.1 kB at 6.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.pom (1.8 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.pom (1.6 kB at 4.0 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.pom (18 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.pom (6.6 kB at 16 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-client/4.5.13/httpcomponents-client-4.5.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-client/4.5.13/httpcomponents-client-4.5.13.pom (16 kB at 33 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-parent/11/httpcomponents-parent-11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-parent/11/httpcomponents-parent-11.pom (35 kB at 69 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.pom (5.0 kB at 12 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-core/4.4.13/httpcomponents-core-4.4.13.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcomponents-core/4.4.13/httpcomponents-core-4.4.13.pom (13 kB at 26 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.pom (19 kB at 39 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/34/commons-parent-34.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-parent/34/commons-parent-34.pom (56 kB at 112 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.pom (14 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.pom (2.3 kB at 4.5 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.pom (5.3 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.pom (815 B at 1.8 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.5/plexus-containers-1.5.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.5/plexus-containers-1.5.5.pom (4.2 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.4/plexus-component-annotations-1.5.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.4/plexus-component-annotations-1.5.4.pom (815 B at 1.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.4/plexus-containers-1.5.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-containers/1.5.4/plexus-containers-1.5.4.pom (4.2 kB at 10 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.5/plexus-2.0.5.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus/2.0.5/plexus-2.0.5.pom (17 kB at 35 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.pom (5.0 kB at 13 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.pom (3.0 kB at 6.1 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.6.3/maven-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven/3.6.3/maven-3.6.3.pom (26 kB at 61 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.6.3/maven-model-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-model/3.6.3/maven-model-3.6.3.pom (4.1 kB at 8.7 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.pom (5.3 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.6.3/maven-artifact-3.6.3.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-artifact/3.6.3/maven-artifact-3.6.3.pom (2.4 kB at 6.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.pom (4.2 kB at 11 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.3.4/sisu-plexus-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-plexus/0.3.4/sisu-plexus-0.3.4.pom (14 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.pom (2.6 kB at 5.4 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.3.4/sisu-inject-0.3.4.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/sisu-inject/0.3.4/sisu-inject-0.3.4.pom (14 kB at 36 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.2/plexus-classworlds-2.5.2.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.5.2/plexus-classworlds-2.5.2.pom (7.3 kB at 18 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.17/plexus-utils-3.0.17.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.0.17/plexus-utils-3.0.17.pom (3.4 kB at 6.9 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.pom
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.pom (7.9 kB at 20 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.jar
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-buildpack-platform/2.4.2/spring-boot-buildpack-platform-2.4.2.jar (202 kB at 225 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/commons/commons-compress/1.20/commons-compress-1.20.jar (632 kB at 383 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpcore/4.4.13/httpcore-4.4.13.jar (329 kB at 168 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar (62 kB at 30 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna/5.5.0/jna-5.5.0.jar (1.5 MB at 607 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/2.4.2/spring-boot-loader-tools-2.4.2.jar (243 kB at 87 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/commons-codec/commons-codec/1.11/commons-codec-1.11.jar (335 kB at 117 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-common-artifact-filters/3.1.0/maven-common-artifact-filters-3.1.0.jar (61 kB at 21 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-component-annotations/1.5.5/plexus-component-annotations-1.5.5.jar (4.2 kB at 1.3 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/maven-plugin-api/3.6.3/maven-plugin-api-3.6.3.jar (47 kB at 14 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/maven/shared/maven-shared-utils/3.1.0/maven-shared-utils-3.1.0.jar (164 kB at 47 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.plexus/0.3.4/org.eclipse.sisu.plexus-0.3.4.jar (205 kB at 54 kB/s)
[INFO] Downloading from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-utils/3.2.1/plexus-utils-3.2.1.jar (262 kB at 62 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/eclipse/sisu/org.eclipse.sisu.inject/0.3.4/org.eclipse.sisu.inject-0.3.4.jar (379 kB at 90 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/codehaus/plexus/plexus-classworlds/2.6.0/plexus-classworlds-2.6.0.jar (53 kB at 13 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/org/apache/httpcomponents/httpclient/4.5.13/httpclient-4.5.13.jar (780 kB at 170 kB/s)
[INFO] Downloaded from central: https://repo1.maven.org/maven2/net/java/dev/jna/jna-platform/5.5.0/jna-platform-5.5.0.jar (2.7 MB at 269 kB/s)
[INFO] Replacing main artifact with repackaged archive
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  03:52 min
[INFO] Finished at: 2022-03-23T03:14:02Z
[INFO] ------------------------------------------------------------------------
[WARNING] The requested profile "openshift" could not be activated because it does not exist.
INFO Copying deployments from target to /deployments...
<b>'/tmp/src/target/spring-hello-1.0.jar' -> '/deployments/spring-hello-1.0.jar'</b>
INFO Cleaning up source directory (/tmp/src)
STEP 9/9: CMD /usr/local/s2i/run
COMMIT temp.builder.openshift.io/default/spring-ms-1:1de69dc5
time="2022-03-23T03:14:02Z" level=warning msg="Adding metacopy option, configured globally"
Getting image source signatures
Copying blob sha256:9317c0d5211ba25c2d4a671312e5d0d5a356fe604760f9d6fd48ac702412d04d
Copying blob sha256:dbbc9ddce71e1fff2fbf111f90ef6374da2788ee88ea55cba811dad4070e41f1
Copying blob sha256:6a21f91ace4c0a257d787f0c9c5d0bf9a3936f38336edb845c0afe9d585ec63b
Copying blob sha256:0690009fe3d8d7b36f80e70dbff471b44f7c777e3a450f5b8832d1763969ceec
Copying config sha256:c5be3bfa5b1be9e0d70b1b48494743186792f71e3d293903fcbebb3f443a117a
Writing manifest to image destination
Storing signatures
--> c5be3bfa5b1
Successfully tagged temp.builder.openshift.io/default/spring-ms-1:1de69dc5
c5be3bfa5b1be9e0d70b1b48494743186792f71e3d293903fcbebb3f443a117a

Pushing image image-registry.openshift-image-registry.svc:5000/default/spring-ms:latest ...
Getting image source signatures
Copying blob sha256:0690009fe3d8d7b36f80e70dbff471b44f7c777e3a450f5b8832d1763969ceec
Copying blob sha256:6a9adb09cd70b25c97460505b84a95a49cf872f814387c67a714738ae4ace009
Copying blob sha256:6a903172bc76e121453f7baf4e5f0ca4127479953db19ecd571f7ce6054057b8
Copying blob sha256:e2cbb7720081a428878d2e437d9e8fb6db8683b662e41096b8337117f87e6e86
Copying config sha256:c5be3bfa5b1be9e0d70b1b48494743186792f71e3d293903fcbebb3f443a117a
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/default/spring-ms@sha256:d7d4eee0ef03d45a70dda75b9185bd4ada073ac417e6b2dd29c383f3b7e2b6a9
Push successful
</pre>


## ⛹️‍♂️ Lab - Listing imagestream in the current project
```
oc get is
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get is</b>
NAME        IMAGE REPOSITORY                                                   TAGS     UPDATED
openjdk     image-registry.openshift-image-registry.svc:5000/jegan/openjdk     latest   16 hours ago
spring-ms   image-registry.openshift-image-registry.svc:5000/jegan/spring-ms   latest   16 hours ago
</pre>

## ⛹️ Lab - Listing imagestreams in all namespaces as kubeadmin
```
oc get is --all-namespaces
```

<pre>
(jegan@tektutor.org)$ <b>oc get is --all-namespaces</b>
NAMESPACE   NAME                                                  IMAGE REPOSITORY                                                                                                 TAGS                                                     UPDATED
jegan       hello-spring-boot                                     image-registry.openshift-image-registry.svc:5000/jegan/hello-spring-boot                                         latest                                                   2 hours ago
openshift   apicast-gateway                                       image-registry.openshift-image-registry.svc:5000/openshift/apicast-gateway                                       2.1.0.GA,2.10.0.GA,2.2.0.GA,2.3.0.GA + 7 more...         3 weeks ago
openshift   apicurito-ui                                          image-registry.openshift-image-registry.svc:5000/openshift/apicurito-ui                                          1.2,1.3,1.4,1.5,1.6,1.7,1.8                              3 weeks ago
openshift   cli                                                   image-registry.openshift-image-registry.svc:5000/openshift/cli                                                   latest                                                   3 weeks ago
openshift   cli-artifacts                                         image-registry.openshift-image-registry.svc:5000/openshift/cli-artifacts                                         latest                                                   3 weeks ago
openshift   dotnet                                                image-registry.openshift-image-registry.svc:5000/openshift/dotnet                                                2.1,2.1-el7,2.1-ubi8,3.1,3.1-el7 + 4 more...             3 weeks ago
openshift   dotnet-runtime                                        image-registry.openshift-image-registry.svc:5000/openshift/dotnet-runtime                                        2.1,2.1-el7,2.1-ubi8,3.1,3.1-el7 + 4 more...             3 weeks ago
openshift   driver-toolkit                                        image-registry.openshift-image-registry.svc:5000/openshift/driver-toolkit                                        49.84.202202141503-0,latest                              3 weeks ago
openshift   eap-cd-openshift                                      image-registry.openshift-image-registry.svc:5000/openshift/eap-cd-openshift                                      12,12.0,13,13.0,14,14.0,15,15.0,16,16.0 + 9 more...      3 weeks ago
openshift   eap-cd-runtime-openshift                              image-registry.openshift-image-registry.svc:5000/openshift/eap-cd-runtime-openshift                              18,18.0,19,19.0,20,20.0,latest                           3 weeks ago
openshift   fis-java-openshift                                    image-registry.openshift-image-registry.svc:5000/openshift/fis-java-openshift                                    1.0,2.0                                                  3 weeks ago
openshift   fis-karaf-openshift                                   image-registry.openshift-image-registry.svc:5000/openshift/fis-karaf-openshift                                   1.0,2.0                                                  3 weeks ago
openshift   fuse-apicurito-generator                              image-registry.openshift-image-registry.svc:5000/openshift/fuse-apicurito-generator                              1.2,1.3,1.4,1.5,1.6,1.7,1.8                              3 weeks ago
openshift   fuse7-console                                         image-registry.openshift-image-registry.svc:5000/openshift/fuse7-console                                         1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8                      3 weeks ago
openshift   fuse7-eap-openshift                                   image-registry.openshift-image-registry.svc:5000/openshift/fuse7-eap-openshift                                   1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8                      3 weeks ago
openshift   fuse7-java-openshift                                  image-registry.openshift-image-registry.svc:5000/openshift/fuse7-java-openshift                                  1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8                      3 weeks ago
openshift   fuse7-karaf-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/fuse7-karaf-openshift                                 1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8                      3 weeks ago
openshift   golang                                                image-registry.openshift-image-registry.svc:5000/openshift/golang                                                1.13.4-ubi7,1.14.7-ubi8,latest                           3 weeks ago
openshift   httpd                                                 image-registry.openshift-image-registry.svc:5000/openshift/httpd                                                 2.4,2.4-el7,2.4-el8,latest                               3 weeks ago
openshift   installer                                             image-registry.openshift-image-registry.svc:5000/openshift/installer                                             latest                                                   3 weeks ago
openshift   installer-artifacts                                   image-registry.openshift-image-registry.svc:5000/openshift/installer-artifacts                                   latest                                                   3 weeks ago
openshift   java                                                  image-registry.openshift-image-registry.svc:5000/openshift/java                                                  11,8,latest,openjdk-11-el7,openjdk-11-ubi8 + 2 more...   3 weeks ago
openshift   jboss-amq-62                                          image-registry.openshift-image-registry.svc:5000/openshift/jboss-amq-62                                          1.1,1.2,1.3,1.4,1.5,1.6,1.7                              3 weeks ago
openshift   jboss-amq-63                                          image-registry.openshift-image-registry.svc:5000/openshift/jboss-amq-63                                          1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
openshift   jboss-datagrid65-client-openshift                     image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid65-client-openshift                     1.0,1.1                                                  3 weeks ago
openshift   jboss-datagrid65-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid65-openshift                            1.2,1.3,1.4,1.5,1.6                                      3 weeks ago
openshift   jboss-datagrid71-client-openshift                     image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid71-client-openshift                     1.0                                                      3 weeks ago
openshift   jboss-datagrid71-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid71-openshift                            1.0,1.1,1.2,1.3                                          3 weeks ago
openshift   jboss-datagrid72-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid72-openshift                            1.0,1.1,1.2                                              3 weeks ago
openshift   jboss-datagrid73-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-datagrid73-openshift                            1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
openshift   jboss-datavirt64-driver-openshift                     image-registry.openshift-image-registry.svc:5000/openshift/jboss-datavirt64-driver-openshift                     1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7                          3 weeks ago
openshift   jboss-datavirt64-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-datavirt64-openshift                            1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7                          3 weeks ago
openshift   jboss-decisionserver64-openshift                      image-registry.openshift-image-registry.svc:5000/openshift/jboss-decisionserver64-openshift                      1.0,1.1,1.2,1.3,1.4,1.5,1.6                              3 weeks ago
openshift   jboss-eap64-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap64-openshift                                 1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8,1.9 + 1 more...          3 weeks ago
openshift   jboss-eap70-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap70-openshift                                 1.3,1.4,1.5,1.6,1.7                                      3 weeks ago
openshift   jboss-eap71-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap71-openshift                                 1.1,1.2,1.3,1.4,latest                                   3 weeks ago
openshift   jboss-eap72-openjdk11-openshift-rhel8                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap72-openjdk11-openshift-rhel8                 1.0,1.1,latest                                           3 weeks ago
openshift   jboss-eap72-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap72-openshift                                 1.0,1.1,1.2,latest                                       3 weeks ago
openshift   jboss-eap73-openjdk11-openshift                       image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openjdk11-openshift                       7.3,7.3.0,latest                                         3 weeks ago
openshift   jboss-eap73-openjdk11-runtime-openshift               image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openjdk11-runtime-openshift               7.3,7.3.0,latest                                         3 weeks ago
openshift   jboss-eap73-openshift                                 image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-openshift                                 7.3,7.3.0,latest                                         3 weeks ago
openshift   jboss-eap73-runtime-openshift                         image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap73-runtime-openshift                         7.3,7.3.0,latest                                         3 weeks ago
openshift   jboss-eap74-openjdk11-openshift                       image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk11-openshift                       7.4.0,latest                                             3 weeks ago
openshift   jboss-eap74-openjdk11-runtime-openshift               image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk11-runtime-openshift               7.4.0,latest                                             3 weeks ago
openshift   jboss-eap74-openjdk8-openshift                        image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk8-openshift                        7.4.0,latest                                             3 weeks ago
openshift   jboss-eap74-openjdk8-runtime-openshift                image-registry.openshift-image-registry.svc:5000/openshift/jboss-eap74-openjdk8-runtime-openshift                7.4.0,latest                                             3 weeks ago
openshift   jboss-fuse70-console                                  image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-console                                  1.0                                                      3 weeks ago
openshift   jboss-fuse70-eap-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-eap-openshift                            1.0                                                      3 weeks ago
openshift   jboss-fuse70-java-openshift                           image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-java-openshift                           1.0                                                      3 weeks ago
openshift   jboss-fuse70-karaf-openshift                          image-registry.openshift-image-registry.svc:5000/openshift/jboss-fuse70-karaf-openshift                          1.0                                                      3 weeks ago
openshift   jboss-processserver64-openshift                       image-registry.openshift-image-registry.svc:5000/openshift/jboss-processserver64-openshift                       1.0,1.1,1.2,1.3,1.4,1.5,1.6                              3 weeks ago
openshift   jboss-webserver31-tomcat7-openshift                   image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver31-tomcat7-openshift                   1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
openshift   jboss-webserver31-tomcat8-openshift                   image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver31-tomcat8-openshift                   1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
openshift   jboss-webserver54-openjdk11-tomcat9-openshift-rhel7   image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver54-openjdk11-tomcat9-openshift-rhel7   1.0,latest                                               3 weeks ago
openshift   jboss-webserver54-openjdk11-tomcat9-openshift-ubi8    image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver54-openjdk11-tomcat9-openshift-ubi8    1.0,latest                                               3 weeks ago
openshift   jboss-webserver54-openjdk8-tomcat9-openshift-rhel7    image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver54-openjdk8-tomcat9-openshift-rhel7    1.0,latest                                               3 weeks ago
openshift   jboss-webserver54-openjdk8-tomcat9-openshift-ubi8     image-registry.openshift-image-registry.svc:5000/openshift/jboss-webserver54-openjdk8-tomcat9-openshift-ubi8     1.0,latest                                               3 weeks ago
openshift   jenkins                                               image-registry.openshift-image-registry.svc:5000/openshift/jenkins                                               2,latest                                                 3 weeks ago
openshift   jenkins-agent-base                                    image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-base                                    latest                                                   3 weeks ago
openshift   jenkins-agent-maven                                   image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-maven                                   latest,v4.0                                              3 weeks ago
openshift   jenkins-agent-nodejs                                  image-registry.openshift-image-registry.svc:5000/openshift/jenkins-agent-nodejs                                  latest,v4.0                                              3 weeks ago
openshift   mariadb                                               image-registry.openshift-image-registry.svc:5000/openshift/mariadb                                               10.3,10.3-el7,10.3-el8,10.5-el7 + 2 more...              3 weeks ago
openshift   must-gather                                           image-registry.openshift-image-registry.svc:5000/openshift/must-gather                                           latest                                                   3 weeks ago
openshift   mysql                                                 image-registry.openshift-image-registry.svc:5000/openshift/mysql                                                 8.0,8.0-el7,8.0-el8,latest                               3 weeks ago
openshift   nginx                                                 image-registry.openshift-image-registry.svc:5000/openshift/nginx                                                 1.16,1.16-el7,1.16-el8,1.18-ubi7 + 2 more...             3 weeks ago
openshift   nodejs                                                image-registry.openshift-image-registry.svc:5000/openshift/nodejs                                                12,12-ubi7,12-ubi8,14-ubi7,14-ubi8 + 2 more...           3 weeks ago
openshift   oauth-proxy                                           image-registry.openshift-image-registry.svc:5000/openshift/oauth-proxy                                           v4.4                                                     3 weeks ago
openshift   openjdk-11-rhel7                                      image-registry.openshift-image-registry.svc:5000/openshift/openjdk-11-rhel7                                      1.0,1.1                                                  3 weeks ago
openshift   openjdk-11-rhel8                                      image-registry.openshift-image-registry.svc:5000/openshift/openjdk-11-rhel8                                      1.0                                                      3 weeks ago
openshift   perl                                                  image-registry.openshift-image-registry.svc:5000/openshift/perl                                                  5.26-ubi8,5.30,5.30-el7,5.30-ubi8 + 1 more...            3 weeks ago
openshift   php                                                   image-registry.openshift-image-registry.svc:5000/openshift/php                                                   7.3,7.3-ubi7,7.3-ubi8,7.4-ubi8,latest                    3 weeks ago
openshift   postgresql                                            image-registry.openshift-image-registry.svc:5000/openshift/postgresql                                            10,10-el7,10-el8,12,12-el7,12-el8 + 4 more...            3 weeks ago
openshift   python                                                image-registry.openshift-image-registry.svc:5000/openshift/python                                                2.7,2.7-ubi7,2.7-ubi8,3.6-ubi8,3.8 + 4 more...           3 weeks ago
openshift   redhat-openjdk18-openshift                            image-registry.openshift-image-registry.svc:5000/openshift/redhat-openjdk18-openshift                            1.0,1.1,1.2,1.3,1.4,1.5,1.6,1.7,1.8                      3 weeks ago
openshift   redhat-sso70-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso70-openshift                                1.3,1.4                                                  3 weeks ago
openshift   redhat-sso71-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso71-openshift                                1.0,1.1,1.2,1.3                                          3 weeks ago
openshift   redhat-sso72-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso72-openshift                                1.0,1.1,1.2,1.3,1.4                                      3 weeks ago
openshift   redhat-sso73-openshift                                image-registry.openshift-image-registry.svc:5000/openshift/redhat-sso73-openshift                                1.0,latest                                               3 weeks ago
openshift   redis                                                 image-registry.openshift-image-registry.svc:5000/openshift/redis                                                 5,5-el7,5-el8,latest                                     3 weeks ago
openshift   rhdm-decisioncentral-rhel8                            image-registry.openshift-image-registry.svc:5000/openshift/rhdm-decisioncentral-rhel8                            7.11.0                                                   3 weeks ago
openshift   rhdm-kieserver-rhel8                                  image-registry.openshift-image-registry.svc:5000/openshift/rhdm-kieserver-rhel8                                  7.11.0                                                   3 weeks ago
openshift   rhpam-businesscentral-monitoring-rhel8                image-registry.openshift-image-registry.svc:5000/openshift/rhpam-businesscentral-monitoring-rhel8                7.11.0                                                   3 weeks ago
openshift   rhpam-businesscentral-rhel8                           image-registry.openshift-image-registry.svc:5000/openshift/rhpam-businesscentral-rhel8                           7.11.0                                                   3 weeks ago
openshift   rhpam-kieserver-rhel8                                 image-registry.openshift-image-registry.svc:5000/openshift/rhpam-kieserver-rhel8                                 7.11.0                                                   3 weeks ago
openshift   rhpam-smartrouter-rhel8                               image-registry.openshift-image-registry.svc:5000/openshift/rhpam-smartrouter-rhel8                               7.11.0                                                   3 weeks ago
openshift   ruby                                                  image-registry.openshift-image-registry.svc:5000/openshift/ruby                                                  2.5-ubi8,2.6,2.6-ubi7,2.6-ubi8,2.7 + 4 more...           3 weeks ago
openshift   sso74-openshift-rhel8                                 image-registry.openshift-image-registry.svc:5000/openshift/sso74-openshift-rhel8                                 7.4,latest                                               3 weeks ago
openshift   tests                                                 image-registry.openshift-image-registry.svc:5000/openshift/tests                                                 latest                                                   3 weeks ago
openshift   tools                                                 image-registry.openshift-image-registry.svc:5000/openshift/tools                                                 latest                                                   3 weeks ago
openshift   ubi8-openjdk-11                                       image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-11                                       1.3                                                      3 weeks ago
openshift   ubi8-openjdk-8                                        image-registry.openshift-image-registry.svc:5000/openshift/ubi8-openjdk-8                                        1.3                                                      3 weeks ago
</pre>

## ⛹️‍♀️ Lab - Delete the existing project to clean-up resources
```
oc delete project jegan
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>

## ⛹️ Lab - Deploying an application using S2I specifying the base openshift builder image

#### Let's create a new project before deploying our application

```
oc new-project jegan
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Already on project "jegan" on server "https://api.tektutor.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname
</pre>

#### Now let's deploy our application from TekTutor GitHub Repository
```
oc new-app java:openjdk-11-el7~https://github.com/tektutor/hello-spring-boot.git
```
The expected output is

<pre>
(jegan@tektutor.org)$ <b>oc new-app java:openjdk-11-el7~https://github.com/tektutor/hello-spring-boot.git</b>
--> Found image 385acdc (2 months old) in image stream "openshift/java" under tag "openjdk-11-el7" for "java:openjdk-11-el7"

    Java Applications 
    ----------------- 
    Platform for building and running plain Java applications (fat-jar and flat classpath)

    Tags: builder, java

    * A source build using source code from https://github.com/tektutor/hello-spring-boot.git will be created
      * The resulting image will be pushed to image stream tag "hello-spring-boot:latest"
      * Use 'oc start-build' to trigger a new build

--> Creating resources ...
    imagestream.image.openshift.io "hello-spring-boot" created
    buildconfig.build.openshift.io "hello-spring-boot" created
    deployment.apps "hello-spring-boot" created
    service "hello-spring-boot" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/hello-spring-boot' to track its progress.
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/hello-spring-boot' 
    Run 'oc status' to view your app.
</pre>

#### Let's us check the status of the application deployment
```
oc status
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc status</b>
In project jegan on server https://api.tektutor.tektutor.org:6443

svc/hello-spring-boot - 172.30.158.204 ports 8080, 8443, 8778
  deployment/hello-spring-boot deploys istag/hello-spring-boot:latest <-
    bc/hello-spring-boot source builds https://github.com/tektutor/hello-spring-boot.git on openshift/java:openjdk-11-el7 
      build #1 running for 3 seconds
    deployment #1 running for 3 seconds - 0/1 pods growing to 1


1 info identified, use 'oc status --suggest' to see details.
</pre>

#### Check the build config
```
oc get bc
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get bc</b>
NAME                TYPE     FROM   LATEST
hello-spring-boot   Source   Git    1
</pre>

#### You may optionally describe the build config to find more details

```
oc describe bc/hello-spring-boot
```

<pre>
(jegan@tektutor.org)$ <b>oc describe bc/hello-spring-boot</b>
Name:		hello-spring-boot
Namespace:	jegan
Created:	14 minutes ago
Labels:		app=hello-spring-boot
		app.kubernetes.io/component=hello-spring-boot
		app.kubernetes.io/instance=hello-spring-boot
Annotations:	openshift.io/generated-by=OpenShiftNewApp
Latest Version:	1

Strategy:	Source
URL:		https://github.com/tektutor/hello-spring-boot.git
From Image:	ImageStreamTag openshift/java:openjdk-11-el7
Volumes:	<none>
Output to:	ImageStreamTag hello-spring-boot:latest

Build Run Policy:	Serial
Triggered by:		Config, ImageChange
Webhook GitHub:
	URL:	https://api.tektutor.tektutor.org:6443/apis/build.openshift.io/v1/namespaces/jegan/buildconfigs/hello-spring-boot/webhooks/<secret>/github
Webhook Generic:
	URL:		https://api.tektutor.tektutor.org:6443/apis/build.openshift.io/v1/namespaces/jegan/buildconfigs/hello-spring-boot/webhooks/<secret>/generic
	AllowEnv:	false
Builds History Limit:
	Successful:	5
	Failed:		5

Build			Status		Duration	Creation Time
hello-spring-boot-1 	complete 	3m53s 		2022-03-23 06:04:02 +0530 IST

Events:
  Type		Reason				Age	From			Message
  ----		------				----	----			-------
  Warning	BuildConfigTriggerFailed	14m	buildconfig-controller	error triggering Build for BuildConfig jegan/hello-spring-boot: Internal error occurred: build config jegan/hello-spring-boot has already instantiated a build for imageid image-registry.openshift-image-registry.svc:5000/openshift/java@sha256:0618d4d6ebc7f40df445063c167d101d12e4955bc60b15713af43bd188104ab5
</pre>

### Let's find the details of the service
```
oc get svc
oc describe svc/hello-spring-boot
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get svc</b>
NAME                TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
hello-spring-boot   ClusterIP   172.30.158.204   <none>        8080/TCP,8443/TCP,8778/TCP   104m
(jegan@tektutor.org)$ <b>oc describe svc/hello-spring-boot</b>
Name:              hello-spring-boot
Namespace:         jegan
Labels:            app=hello-spring-boot
                   app.kubernetes.io/component=hello-spring-boot
                   app.kubernetes.io/instance=hello-spring-boot
Annotations:       openshift.io/generated-by: OpenShiftNewApp
Selector:          deployment=hello-spring-boot
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                172.30.158.204
IPs:               172.30.158.204
Port:              8080-tcp  8080/TCP
TargetPort:        8080/TCP
Endpoints:         10.128.3.153:8080
Port:              8443-tcp  8443/TCP
TargetPort:        8443/TCP
Endpoints:         10.128.3.153:8443
Port:              8778-tcp  8778/TCP
TargetPort:        8778/TCP
Endpoints:         10.128.3.153:8778
Session Affinity:  None
Events:            <none>
</pre>

#### Let's create a public route for the service
```
oc expose svc/hello-springboot-app
```

#### List the route
```
oc get route
oc describe route/hello-springboot-app
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get route</b>
NAME                HOST/PORT                                            PATH   SERVICES            PORT       TERMINATION   WILDCARD
hello-spring-boot   hello-spring-boot-jegan.apps.tektutor.tektutor.org          hello-spring-boot   8080-tcp                 None
(jegan@tektutor.org)$ <b>curl hello-spring-boot-jegan.apps.tektutor.tektutor.org</b>
Greetings from Spring Boot!
</pre>

## ⛹️‍♂️ Lab - Deploying mysql db server using Docker Image with parameters
```
oc new-app docker.io/mysql:latest -e MYSQL_ROOT_PASSWORD=root
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-app docker.io/mysql:latest -e MYSQL_ROOT_PASSWORD=root</b>
--> Found container image 562c9bc (4 days old) from docker.io for "docker.io/mysql:latest"

    * An image stream tag will be created as "mysql:latest" that will track this image

--> Creating resources ...
    imagestream.image.openshift.io "mysql" created
    deployment.apps "mysql" created
    service "mysql" created
--> Success
    Application is not exposed. You can expose services to the outside world by executing one or more of the commands below:
     'oc expose service/mysql' 
    Run 'oc status' to view your app.
</pre>

Check the status of the application deployment
```
oc status
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc status</b>
In project jegan on server https://api.tektutor.tektutor.org:6443

svc/mysql - 172.30.116.98 ports 3306, 33060
  deployment/mysql deploys istag/mysql:latest 
    deployment #2 running for 2 seconds - 0/1 pods
    deployment #1 deployed 7 seconds ago - 0/1 pods growing to 1


1 info identified, use 'oc status --suggest' to see details.
(jegan@tektutor.org)$ <b>oc status</b>
In project jegan on server https://api.tektutor.tektutor.org:6443

svc/mysql - 172.30.116.98 ports 3306, 33060
  deployment/mysql deploys istag/mysql:latest 
    deployment #2 running for 6 seconds - 1 pod
    deployment #1 deployed 11 seconds ago


1 info identified, use 'oc status --suggest' to see details.
</pre>

Check if the mysql deployment is ready
```
oc get deploy
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get deploy</b>
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
mysql   1/1     1            1           24s
</pre>

Check the pods
```
oc get po
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get po</b>
NAME                     READY   STATUS    RESTARTS   AGE
mysql-6659459f68-hkjs4   1/1     Running   0          10s
</pre>

Let's get inside the mysql Pod shell
```
oc exec -it mysql-6659459f68-hkjs4 sh
mysql -u root -p
```
When prompted for mysql root password, type 'root' without quotes as the password.

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc exec -it mysql-6659459f68-hkjs4 sh</b>
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.28 MySQL Community Server - GPL

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> exit
Bye
$ exit
</pre>

# Session - 4

## List the API resources
```
oc api-resources
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc api-resources</b>
NAME                                  SHORTNAMES       APIVERSION                                    NAMESPACED   KIND
bindings                                               v1                                            true         Binding
componentstatuses                     cs               v1                                            false        ComponentStatus
configmaps                            cm               v1                                            true         ConfigMap
endpoints                             ep               v1                                            true         Endpoints
events                                ev               v1                                            true         Event
limitranges                           limits           v1                                            true         LimitRange
namespaces                            ns               v1                                            false        Namespace
nodes                                 no               v1                                            false        Node
persistentvolumeclaims                pvc              v1                                            true         PersistentVolumeClaim
persistentvolumes                     pv               v1                                            false        PersistentVolume
pods                                  po               v1                                            true         Pod
podtemplates                                           v1                                            true         PodTemplate
replicationcontrollers                rc               v1                                            true         ReplicationController
resourcequotas                        quota            v1                                            true         ResourceQuota
secrets                                                v1                                            true         Secret
serviceaccounts                       sa               v1                                            true         ServiceAccount
services                              svc              v1                                            true         Service
mutatingwebhookconfigurations                          admissionregistration.k8s.io/v1               false        MutatingWebhookConfiguration
validatingwebhookconfigurations                        admissionregistration.k8s.io/v1               false        ValidatingWebhookConfiguration
customresourcedefinitions             crd,crds         apiextensions.k8s.io/v1                       false        CustomResourceDefinition
apiservices                                            apiregistration.k8s.io/v1                     false        APIService
apirequestcounts                                       apiserver.openshift.io/v1                     false        APIRequestCount
controllerrevisions                                    apps/v1                                       true         ControllerRevision
daemonsets                            ds               apps/v1                                       true         DaemonSet
deployments                           deploy           apps/v1                                       true         Deployment
replicasets                           rs               apps/v1                                       true         ReplicaSet
statefulsets                          sts              apps/v1                                       true         StatefulSet
deploymentconfigs                     dc               apps.openshift.io/v1                          true         DeploymentConfig
tokenreviews                                           authentication.k8s.io/v1                      false        TokenReview
localsubjectaccessreviews                              authorization.k8s.io/v1                       true         LocalSubjectAccessReview
selfsubjectaccessreviews                               authorization.k8s.io/v1                       false        SelfSubjectAccessReview
selfsubjectrulesreviews                                authorization.k8s.io/v1                       false        SelfSubjectRulesReview
subjectaccessreviews                                   authorization.k8s.io/v1                       false        SubjectAccessReview
clusterrolebindings                                    authorization.openshift.io/v1                 false        ClusterRoleBinding
clusterroles                                           authorization.openshift.io/v1                 false        ClusterRole
localresourceaccessreviews                             authorization.openshift.io/v1                 true         LocalResourceAccessReview
localsubjectaccessreviews                              authorization.openshift.io/v1                 true         LocalSubjectAccessReview
resourceaccessreviews                                  authorization.openshift.io/v1                 false        ResourceAccessReview
rolebindingrestrictions                                authorization.openshift.io/v1                 true         RoleBindingRestriction
rolebindings                                           authorization.openshift.io/v1                 true         RoleBinding
roles                                                  authorization.openshift.io/v1                 true         Role
selfsubjectrulesreviews                                authorization.openshift.io/v1                 true         SelfSubjectRulesReview
subjectaccessreviews                                   authorization.openshift.io/v1                 false        SubjectAccessReview
subjectrulesreviews                                    authorization.openshift.io/v1                 true         SubjectRulesReview
horizontalpodautoscalers              hpa              autoscaling/v1                                true         HorizontalPodAutoscaler
clusterautoscalers                    ca               autoscaling.openshift.io/v1                   false        ClusterAutoscaler
machineautoscalers                    ma               autoscaling.openshift.io/v1beta1              true         MachineAutoscaler
cronjobs                              cj               batch/v1                                      true         CronJob
jobs                                                   batch/v1                                      true         Job
buildconfigs                          bc               build.openshift.io/v1                         true         BuildConfig
builds                                                 build.openshift.io/v1                         true         Build
certificatesigningrequests            csr              certificates.k8s.io/v1                        false        CertificateSigningRequest
credentialsrequests                                    cloudcredential.openshift.io/v1               true         CredentialsRequest
apiservers                                             config.openshift.io/v1                        false        APIServer
authentications                                        config.openshift.io/v1                        false        Authentication
builds                                                 config.openshift.io/v1                        false        Build
clusteroperators                      co               config.openshift.io/v1                        false        ClusterOperator
clusterversions                                        config.openshift.io/v1                        false        ClusterVersion
consoles                                               config.openshift.io/v1                        false        Console
dnses                                                  config.openshift.io/v1                        false        DNS
featuregates                                           config.openshift.io/v1                        false        FeatureGate
images                                                 config.openshift.io/v1                        false        Image
infrastructures                                        config.openshift.io/v1                        false        Infrastructure
ingresses                                              config.openshift.io/v1                        false        Ingress
networks                                               config.openshift.io/v1                        false        Network
oauths                                                 config.openshift.io/v1                        false        OAuth
operatorhubs                                           config.openshift.io/v1                        false        OperatorHub
projects                                               config.openshift.io/v1                        false        Project
proxies                                                config.openshift.io/v1                        false        Proxy
schedulers                                             config.openshift.io/v1                        false        Scheduler
consoleclidownloads                                    console.openshift.io/v1                       false        ConsoleCLIDownload
consoleexternalloglinks                                console.openshift.io/v1                       false        ConsoleExternalLogLink
consolelinks                                           console.openshift.io/v1                       false        ConsoleLink
consolenotifications                                   console.openshift.io/v1                       false        ConsoleNotification
consoleplugins                                         console.openshift.io/v1alpha1                 false        ConsolePlugin
consolequickstarts                                     console.openshift.io/v1                       false        ConsoleQuickStart
consoleyamlsamples                                     console.openshift.io/v1                       false        ConsoleYAMLSample
podnetworkconnectivitychecks                           controlplane.operator.openshift.io/v1alpha1   true         PodNetworkConnectivityCheck
leases                                                 coordination.k8s.io/v1                        true         Lease
endpointslices                                         discovery.k8s.io/v1                           true         EndpointSlice
events                                ev               events.k8s.io/v1                              true         Event
flowschemas                                            flowcontrol.apiserver.k8s.io/v1beta1          false        FlowSchema
prioritylevelconfigurations                            flowcontrol.apiserver.k8s.io/v1beta1          false        PriorityLevelConfiguration
helmchartrepositories                                  helm.openshift.io/v1beta1                     false        HelmChartRepository
images                                                 image.openshift.io/v1                         false        Image
imagesignatures                                        image.openshift.io/v1                         false        ImageSignature
imagestreamimages                     isimage          image.openshift.io/v1                         true         ImageStreamImage
imagestreamimports                                     image.openshift.io/v1                         true         ImageStreamImport
imagestreammappings                                    image.openshift.io/v1                         true         ImageStreamMapping
imagestreams                          is               image.openshift.io/v1                         true         ImageStream
imagestreamtags                       istag            image.openshift.io/v1                         true         ImageStreamTag
imagetags                             itag             image.openshift.io/v1                         true         ImageTag
configs                                                imageregistry.operator.openshift.io/v1        false        Config
imagepruners                                           imageregistry.operator.openshift.io/v1        false        ImagePruner
dnsrecords                                             ingress.operator.openshift.io/v1              true         DNSRecord
network-attachment-definitions        net-attach-def   k8s.cni.cncf.io/v1                            true         NetworkAttachmentDefinition
machinehealthchecks                   mhc,mhcs         machine.openshift.io/v1beta1                  true         MachineHealthCheck
machines                                               machine.openshift.io/v1beta1                  true         Machine
machinesets                                            machine.openshift.io/v1beta1                  true         MachineSet
containerruntimeconfigs               ctrcfg           machineconfiguration.openshift.io/v1          false        ContainerRuntimeConfig
controllerconfigs                                      machineconfiguration.openshift.io/v1          false        ControllerConfig
kubeletconfigs                                         machineconfiguration.openshift.io/v1          false        KubeletConfig
machineconfigpools                    mcp              machineconfiguration.openshift.io/v1          false        MachineConfigPool
machineconfigs                        mc               machineconfiguration.openshift.io/v1          false        MachineConfig
baremetalhosts                        bmh,bmhost       metal3.io/v1alpha1                            true         BareMetalHost
provisionings                                          metal3.io/v1alpha1                            false        Provisioning
nodes                                                  metrics.k8s.io/v1beta1                        false        NodeMetrics
pods                                                   metrics.k8s.io/v1beta1                        true         PodMetrics
storagestates                                          migration.k8s.io/v1alpha1                     false        StorageState
storageversionmigrations                               migration.k8s.io/v1alpha1                     false        StorageVersionMigration
alertmanagerconfigs                                    monitoring.coreos.com/v1alpha1                true         AlertmanagerConfig
alertmanagers                                          monitoring.coreos.com/v1                      true         Alertmanager
podmonitors                                            monitoring.coreos.com/v1                      true         PodMonitor
probes                                                 monitoring.coreos.com/v1                      true         Probe
prometheuses                                           monitoring.coreos.com/v1                      true         Prometheus
prometheusrules                                        monitoring.coreos.com/v1                      true         PrometheusRule
servicemonitors                                        monitoring.coreos.com/v1                      true         ServiceMonitor
thanosrulers                                           monitoring.coreos.com/v1                      true         ThanosRuler
clusternetworks                                        network.openshift.io/v1                       false        ClusterNetwork
egressnetworkpolicies                                  network.openshift.io/v1                       true         EgressNetworkPolicy
hostsubnets                                            network.openshift.io/v1                       false        HostSubnet
netnamespaces                                          network.openshift.io/v1                       false        NetNamespace
egressrouters                                          network.operator.openshift.io/v1              true         EgressRouter
operatorpkis                                           network.operator.openshift.io/v1              true         OperatorPKI
ingressclasses                                         networking.k8s.io/v1                          false        IngressClass
ingresses                             ing              networking.k8s.io/v1                          true         Ingress
networkpolicies                       netpol           networking.k8s.io/v1                          true         NetworkPolicy
runtimeclasses                                         node.k8s.io/v1                                false        RuntimeClass
oauthaccesstokens                                      oauth.openshift.io/v1                         false        OAuthAccessToken
oauthauthorizetokens                                   oauth.openshift.io/v1                         false        OAuthAuthorizeToken
oauthclientauthorizations                              oauth.openshift.io/v1                         false        OAuthClientAuthorization
oauthclients                                           oauth.openshift.io/v1                         false        OAuthClient
tokenreviews                                           oauth.openshift.io/v1                         false        TokenReview
useroauthaccesstokens                                  oauth.openshift.io/v1                         false        UserOAuthAccessToken
authentications                                        operator.openshift.io/v1                      false        Authentication
cloudcredentials                                       operator.openshift.io/v1                      false        CloudCredential
clustercsidrivers                                      operator.openshift.io/v1                      false        ClusterCSIDriver
configs                                                operator.openshift.io/v1                      false        Config
consoles                                               operator.openshift.io/v1                      false        Console
csisnapshotcontrollers                                 operator.openshift.io/v1                      false        CSISnapshotController
dnses                                                  operator.openshift.io/v1                      false        DNS
etcds                                                  operator.openshift.io/v1                      false        Etcd
imagecontentsourcepolicies                             operator.openshift.io/v1alpha1                false        ImageContentSourcePolicy
ingresscontrollers                                     operator.openshift.io/v1                      true         IngressController
kubeapiservers                                         operator.openshift.io/v1                      false        KubeAPIServer
kubecontrollermanagers                                 operator.openshift.io/v1                      false        KubeControllerManager
kubeschedulers                                         operator.openshift.io/v1                      false        KubeScheduler
kubestorageversionmigrators                            operator.openshift.io/v1                      false        KubeStorageVersionMigrator
networks                                               operator.openshift.io/v1                      false        Network
openshiftapiservers                                    operator.openshift.io/v1                      false        OpenShiftAPIServer
openshiftcontrollermanagers                            operator.openshift.io/v1                      false        OpenShiftControllerManager
servicecas                                             operator.openshift.io/v1                      false        ServiceCA
storages                                               operator.openshift.io/v1                      false        Storage
catalogsources                        catsrc           operators.coreos.com/v1alpha1                 true         CatalogSource
clusterserviceversions                csv,csvs         operators.coreos.com/v1alpha1                 true         ClusterServiceVersion
installplans                          ip               operators.coreos.com/v1alpha1                 true         InstallPlan
operatorconditions                    condition        operators.coreos.com/v2                       true         OperatorCondition
operatorgroups                        og               operators.coreos.com/v1                       true         OperatorGroup
operators                                              operators.coreos.com/v1                       false        Operator
subscriptions                         sub,subs         operators.coreos.com/v1alpha1                 true         Subscription
packagemanifests                                       packages.operators.coreos.com/v1              true         PackageManifest
poddisruptionbudgets                  pdb              policy/v1                                     true         PodDisruptionBudget
podsecuritypolicies                   psp              policy/v1beta1                                false        PodSecurityPolicy
projectrequests                                        project.openshift.io/v1                       false        ProjectRequest
projects                                               project.openshift.io/v1                       false        Project
appliedclusterresourcequotas                           quota.openshift.io/v1                         true         AppliedClusterResourceQuota
clusterresourcequotas                 clusterquota     quota.openshift.io/v1                         false        ClusterResourceQuota
clusterrolebindings                                    rbac.authorization.k8s.io/v1                  false        ClusterRoleBinding
clusterroles                                           rbac.authorization.k8s.io/v1                  false        ClusterRole
rolebindings                                           rbac.authorization.k8s.io/v1                  true         RoleBinding
roles                                                  rbac.authorization.k8s.io/v1                  true         Role
routes                                                 route.openshift.io/v1                         true         Route
configs                                                samples.operator.openshift.io/v1              false        Config
priorityclasses                       pc               scheduling.k8s.io/v1                          false        PriorityClass
rangeallocations                                       security.internal.openshift.io/v1             false        RangeAllocation
podsecuritypolicyreviews                               security.openshift.io/v1                      true         PodSecurityPolicyReview
podsecuritypolicyselfsubjectreviews                    security.openshift.io/v1                      true         PodSecurityPolicySelfSubjectReview
podsecuritypolicysubjectreviews                        security.openshift.io/v1                      true         PodSecurityPolicySubjectReview
rangeallocations                                       security.openshift.io/v1                      false        RangeAllocation
securitycontextconstraints            scc              security.openshift.io/v1                      false        SecurityContextConstraints
volumesnapshotclasses                                  snapshot.storage.k8s.io/v1                    false        VolumeSnapshotClass
volumesnapshotcontents                                 snapshot.storage.k8s.io/v1                    false        VolumeSnapshotContent
volumesnapshots                                        snapshot.storage.k8s.io/v1                    true         VolumeSnapshot
csidrivers                                             storage.k8s.io/v1                             false        CSIDriver
csinodes                                               storage.k8s.io/v1                             false        CSINode
csistoragecapacities                                   storage.k8s.io/v1beta1                        true         CSIStorageCapacity
storageclasses                        sc               storage.k8s.io/v1                             false        StorageClass
volumeattachments                                      storage.k8s.io/v1                             false        VolumeAttachment
clustertasks                                           tekton.dev/v1beta1                            false        ClusterTask
conditions                                             tekton.dev/v1alpha1                           true         Condition
pipelineresources                                      tekton.dev/v1alpha1                           true         PipelineResource
pipelineruns                          pr,prs           tekton.dev/v1beta1                            true         PipelineRun
pipelines                                              tekton.dev/v1beta1                            true         Pipeline
runs                                                   tekton.dev/v1alpha1                           true         Run
taskruns                              tr,trs           tekton.dev/v1beta1                            true         TaskRun
tasks                                                  tekton.dev/v1beta1                            true         Task
brokertemplateinstances                                template.openshift.io/v1                      false        BrokerTemplateInstance
processedtemplates                                     template.openshift.io/v1                      true         Template
templateinstances                                      template.openshift.io/v1                      true         TemplateInstance
templates                                              template.openshift.io/v1                      true         Template
clusterinterceptors                   ci               triggers.tekton.dev/v1alpha1                  false        ClusterInterceptor
clustertriggerbindings                ctb              triggers.tekton.dev/v1beta1                   false        ClusterTriggerBinding
eventlisteners                        el               triggers.tekton.dev/v1beta1                   true         EventListener
triggerbindings                       tb               triggers.tekton.dev/v1beta1                   true         TriggerBinding
triggers                              tri              triggers.tekton.dev/v1beta1                   true         Trigger
triggertemplates                      tt               triggers.tekton.dev/v1beta1                   true         TriggerTemplate
profiles                                               tuned.openshift.io/v1                         true         Profile
tuneds                                                 tuned.openshift.io/v1                         true         Tuned
groups                                                 user.openshift.io/v1                          false        Group
identities                                             user.openshift.io/v1                          false        Identity
useridentitymappings                                   user.openshift.io/v1                          false        UserIdentityMapping
users                                                  user.openshift.io/v1                          false        User
ippools                                                whereabouts.cni.cncf.io/v1alpha1              true         IPPool
overlappingrangeipreservations                         whereabouts.cni.cncf.io/v1alpha1              true         OverlappingRangeIPReservation
</pre>

## List API Resources with more information
```
oc api-resources -o wide
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc api-resources -o wide</b>
NAME                                  SHORTNAMES       APIVERSION                                    NAMESPACED   KIND                                 VERBS
bindings                                               v1                                            true         Binding                              [create]
componentstatuses                     cs               v1                                            false        ComponentStatus                      [get list]
configmaps                            cm               v1                                            true         ConfigMap                            [create delete deletecollection get list patch update watch]
endpoints                             ep               v1                                            true         Endpoints                            [create delete deletecollection get list patch update watch]
events                                ev               v1                                            true         Event                                [create delete deletecollection get list patch update watch]
limitranges                           limits           v1                                            true         LimitRange                           [create delete deletecollection get list patch update watch]
namespaces                            ns               v1                                            false        Namespace                            [create delete get list patch update watch]
nodes                                 no               v1                                            false        Node                                 [create delete deletecollection get list patch update watch]
persistentvolumeclaims                pvc              v1                                            true         PersistentVolumeClaim                [create delete deletecollection get list patch update watch]
persistentvolumes                     pv               v1                                            false        PersistentVolume                     [create delete deletecollection get list patch update watch]
pods                                  po               v1                                            true         Pod                                  [create delete deletecollection get list patch update watch]
podtemplates                                           v1                                            true         PodTemplate                          [create delete deletecollection get list patch update watch]
replicationcontrollers                rc               v1                                            true         ReplicationController                [create delete deletecollection get list patch update watch]
resourcequotas                        quota            v1                                            true         ResourceQuota                        [create delete deletecollection get list patch update watch]
secrets                                                v1                                            true         Secret                               [create delete deletecollection get list patch update watch]
serviceaccounts                       sa               v1                                            true         ServiceAccount                       [create delete deletecollection get list patch update watch]
services                              svc              v1                                            true         Service                              [create delete get list patch update watch]
mutatingwebhookconfigurations                          admissionregistration.k8s.io/v1               false        MutatingWebhookConfiguration         [create delete deletecollection get list patch update watch]
validatingwebhookconfigurations                        admissionregistration.k8s.io/v1               false        ValidatingWebhookConfiguration       [create delete deletecollection get list patch update watch]
customresourcedefinitions             crd,crds         apiextensions.k8s.io/v1                       false        CustomResourceDefinition             [create delete deletecollection get list patch update watch]
apiservices                                            apiregistration.k8s.io/v1                     false        APIService                           [create delete deletecollection get list patch update watch]
apirequestcounts                                       apiserver.openshift.io/v1                     false        APIRequestCount                      [delete deletecollection get list patch create update watch]
controllerrevisions                                    apps/v1                                       true         ControllerRevision                   [create delete deletecollection get list patch update watch]
daemonsets                            ds               apps/v1                                       true         DaemonSet                            [create delete deletecollection get list patch update watch]
deployments                           deploy           apps/v1                                       true         Deployment                           [create delete deletecollection get list patch update watch]
replicasets                           rs               apps/v1                                       true         ReplicaSet                           [create delete deletecollection get list patch update watch]
statefulsets                          sts              apps/v1                                       true         StatefulSet                          [create delete deletecollection get list patch update watch]
deploymentconfigs                     dc               apps.openshift.io/v1                          true         DeploymentConfig                     [create delete deletecollection get list patch update watch]
tokenreviews                                           authentication.k8s.io/v1                      false        TokenReview                          [create]
localsubjectaccessreviews                              authorization.k8s.io/v1                       true         LocalSubjectAccessReview             [create]
selfsubjectaccessreviews                               authorization.k8s.io/v1                       false        SelfSubjectAccessReview              [create]
selfsubjectrulesreviews                                authorization.k8s.io/v1                       false        SelfSubjectRulesReview               [create]
subjectaccessreviews                                   authorization.k8s.io/v1                       false        SubjectAccessReview                  [create]
clusterrolebindings                                    authorization.openshift.io/v1                 false        ClusterRoleBinding                   [create delete get list patch update]
clusterroles                                           authorization.openshift.io/v1                 false        ClusterRole                          [create delete get list patch update]
localresourceaccessreviews                             authorization.openshift.io/v1                 true         LocalResourceAccessReview            [create]
localsubjectaccessreviews                              authorization.openshift.io/v1                 true         LocalSubjectAccessReview             [create]
resourceaccessreviews                                  authorization.openshift.io/v1                 false        ResourceAccessReview                 [create]
rolebindingrestrictions                                authorization.openshift.io/v1                 true         RoleBindingRestriction               [create delete deletecollection get list patch update watch]
rolebindings                                           authorization.openshift.io/v1                 true         RoleBinding                          [create delete get list patch update]
roles                                                  authorization.openshift.io/v1                 true         Role                                 [create delete get list patch update]
selfsubjectrulesreviews                                authorization.openshift.io/v1                 true         SelfSubjectRulesReview               [create]
subjectaccessreviews                                   authorization.openshift.io/v1                 false        SubjectAccessReview                  [create]
subjectrulesreviews                                    authorization.openshift.io/v1                 true         SubjectRulesReview                   [create]
horizontalpodautoscalers              hpa              autoscaling/v1                                true         HorizontalPodAutoscaler              [create delete deletecollection get list patch update watch]
clusterautoscalers                    ca               autoscaling.openshift.io/v1                   false        ClusterAutoscaler                    [delete deletecollection get list patch create update watch]
machineautoscalers                    ma               autoscaling.openshift.io/v1beta1              true         MachineAutoscaler                    [delete deletecollection get list patch create update watch]
cronjobs                              cj               batch/v1                                      true         CronJob                              [create delete deletecollection get list patch update watch]
jobs                                                   batch/v1                                      true         Job                                  [create delete deletecollection get list patch update watch]
buildconfigs                          bc               build.openshift.io/v1                         true         BuildConfig                          [create delete deletecollection get list patch update watch]
builds                                                 build.openshift.io/v1                         true         Build                                [create delete deletecollection get list patch update watch]
certificatesigningrequests            csr              certificates.k8s.io/v1                        false        CertificateSigningRequest            [create delete deletecollection get list patch update watch]
credentialsrequests                                    cloudcredential.openshift.io/v1               true         CredentialsRequest                   [delete deletecollection get list patch create update watch]
apiservers                                             config.openshift.io/v1                        false        APIServer                            [delete deletecollection get list patch create update watch]
authentications                                        config.openshift.io/v1                        false        Authentication                       [delete deletecollection get list patch create update watch]
builds                                                 config.openshift.io/v1                        false        Build                                [delete deletecollection get list patch create update watch]
clusteroperators                      co               config.openshift.io/v1                        false        ClusterOperator                      [delete deletecollection get list patch create update watch]
clusterversions                                        config.openshift.io/v1                        false        ClusterVersion                       [delete deletecollection get list patch create update watch]
consoles                                               config.openshift.io/v1                        false        Console                              [delete deletecollection get list patch create update watch]
dnses                                                  config.openshift.io/v1                        false        DNS                                  [delete deletecollection get list patch create update watch]
featuregates                                           config.openshift.io/v1                        false        FeatureGate                          [delete deletecollection get list patch create update watch]
images                                                 config.openshift.io/v1                        false        Image                                [delete deletecollection get list patch create update watch]
infrastructures                                        config.openshift.io/v1                        false        Infrastructure                       [delete deletecollection get list patch create update watch]
ingresses                                              config.openshift.io/v1                        false        Ingress                              [delete deletecollection get list patch create update watch]
networks                                               config.openshift.io/v1                        false        Network                              [delete deletecollection get list patch create update watch]
oauths                                                 config.openshift.io/v1                        false        OAuth                                [delete deletecollection get list patch create update watch]
operatorhubs                                           config.openshift.io/v1                        false        OperatorHub                          [delete deletecollection get list patch create update watch]
projects                                               config.openshift.io/v1                        false        Project                              [delete deletecollection get list patch create update watch]
proxies                                                config.openshift.io/v1                        false        Proxy                                [delete deletecollection get list patch create update watch]
schedulers                                             config.openshift.io/v1                        false        Scheduler                            [delete deletecollection get list patch create update watch]
consoleclidownloads                                    console.openshift.io/v1                       false        ConsoleCLIDownload                   [delete deletecollection get list patch create update watch]
consoleexternalloglinks                                console.openshift.io/v1                       false        ConsoleExternalLogLink               [delete deletecollection get list patch create update watch]
consolelinks                                           console.openshift.io/v1                       false        ConsoleLink                          [delete deletecollection get list patch create update watch]
consolenotifications                                   console.openshift.io/v1                       false        ConsoleNotification                  [delete deletecollection get list patch create update watch]
consoleplugins                                         console.openshift.io/v1alpha1                 false        ConsolePlugin                        [delete deletecollection get list patch create update watch]
consolequickstarts                                     console.openshift.io/v1                       false        ConsoleQuickStart                    [delete deletecollection get list patch create update watch]
consoleyamlsamples                                     console.openshift.io/v1                       false        ConsoleYAMLSample                    [delete deletecollection get list patch create update watch]
podnetworkconnectivitychecks                           controlplane.operator.openshift.io/v1alpha1   true         PodNetworkConnectivityCheck          [delete deletecollection get list patch create update watch]
leases                                                 coordination.k8s.io/v1                        true         Lease                                [create delete deletecollection get list patch update watch]
endpointslices                                         discovery.k8s.io/v1                           true         EndpointSlice                        [create delete deletecollection get list patch update watch]
events                                ev               events.k8s.io/v1                              true         Event                                [create delete deletecollection get list patch update watch]
flowschemas                                            flowcontrol.apiserver.k8s.io/v1beta1          false        FlowSchema                           [create delete deletecollection get list patch update watch]
prioritylevelconfigurations                            flowcontrol.apiserver.k8s.io/v1beta1          false        PriorityLevelConfiguration           [create delete deletecollection get list patch update watch]
helmchartrepositories                                  helm.openshift.io/v1beta1                     false        HelmChartRepository                  [delete deletecollection get list patch create update watch]
images                                                 image.openshift.io/v1                         false        Image                                [create delete deletecollection get list patch update watch]
imagesignatures                                        image.openshift.io/v1                         false        ImageSignature                       [create delete]
imagestreamimages                     isimage          image.openshift.io/v1                         true         ImageStreamImage                     [get]
imagestreamimports                                     image.openshift.io/v1                         true         ImageStreamImport                    [create]
imagestreammappings                                    image.openshift.io/v1                         true         ImageStreamMapping                   [create]
imagestreams                          is               image.openshift.io/v1                         true         ImageStream                          [create delete deletecollection get list patch update watch]
imagestreamtags                       istag            image.openshift.io/v1                         true         ImageStreamTag                       [create delete get list patch update]
imagetags                             itag             image.openshift.io/v1                         true         ImageTag                             [create delete get list patch update]
configs                                                imageregistry.operator.openshift.io/v1        false        Config                               [delete deletecollection get list patch create update watch]
imagepruners                                           imageregistry.operator.openshift.io/v1        false        ImagePruner                          [delete deletecollection get list patch create update watch]
dnsrecords                                             ingress.operator.openshift.io/v1              true         DNSRecord                            [delete deletecollection get list patch create update watch]
network-attachment-definitions        net-attach-def   k8s.cni.cncf.io/v1                            true         NetworkAttachmentDefinition          [delete deletecollection get list patch create update watch]
machinehealthchecks                   mhc,mhcs         machine.openshift.io/v1beta1                  true         MachineHealthCheck                   [delete deletecollection get list patch create update watch]
machines                                               machine.openshift.io/v1beta1                  true         Machine                              [delete deletecollection get list patch create update watch]
machinesets                                            machine.openshift.io/v1beta1                  true         MachineSet                           [delete deletecollection get list patch create update watch]
containerruntimeconfigs               ctrcfg           machineconfiguration.openshift.io/v1          false        ContainerRuntimeConfig               [delete deletecollection get list patch create update watch]
controllerconfigs                                      machineconfiguration.openshift.io/v1          false        ControllerConfig                     [delete deletecollection get list patch create update watch]
kubeletconfigs                                         machineconfiguration.openshift.io/v1          false        KubeletConfig                        [delete deletecollection get list patch create update watch]
machineconfigpools                    mcp              machineconfiguration.openshift.io/v1          false        MachineConfigPool                    [delete deletecollection get list patch create update watch]
machineconfigs                        mc               machineconfiguration.openshift.io/v1          false        MachineConfig                        [delete deletecollection get list patch create update watch]
baremetalhosts                        bmh,bmhost       metal3.io/v1alpha1                            true         BareMetalHost                        [delete deletecollection get list patch create update watch]
provisionings                                          metal3.io/v1alpha1                            false        Provisioning                         [delete deletecollection get list patch create update watch]
nodes                                                  metrics.k8s.io/v1beta1                        false        NodeMetrics                          [get list]
pods                                                   metrics.k8s.io/v1beta1                        true         PodMetrics                           [get list]
storagestates                                          migration.k8s.io/v1alpha1                     false        StorageState                         [delete deletecollection get list patch create update watch]
storageversionmigrations                               migration.k8s.io/v1alpha1                     false        StorageVersionMigration              [delete deletecollection get list patch create update watch]
alertmanagerconfigs                                    monitoring.coreos.com/v1alpha1                true         AlertmanagerConfig                   [delete deletecollection get list patch create update watch]
alertmanagers                                          monitoring.coreos.com/v1                      true         Alertmanager                         [delete deletecollection get list patch create update watch]
podmonitors                                            monitoring.coreos.com/v1                      true         PodMonitor                           [delete deletecollection get list patch create update watch]
probes                                                 monitoring.coreos.com/v1                      true         Probe                                [delete deletecollection get list patch create update watch]
prometheuses                                           monitoring.coreos.com/v1                      true         Prometheus                           [delete deletecollection get list patch create update watch]
prometheusrules                                        monitoring.coreos.com/v1                      true         PrometheusRule                       [delete deletecollection get list patch create update watch]
servicemonitors                                        monitoring.coreos.com/v1                      true         ServiceMonitor                       [delete deletecollection get list patch create update watch]
thanosrulers                                           monitoring.coreos.com/v1                      true         ThanosRuler                          [delete deletecollection get list patch create update watch]
clusternetworks                                        network.openshift.io/v1                       false        ClusterNetwork                       [delete deletecollection get list patch create update watch]
egressnetworkpolicies                                  network.openshift.io/v1                       true         EgressNetworkPolicy                  [delete deletecollection get list patch create update watch]
hostsubnets                                            network.openshift.io/v1                       false        HostSubnet                           [delete deletecollection get list patch create update watch]
netnamespaces                                          network.openshift.io/v1                       false        NetNamespace                         [delete deletecollection get list patch create update watch]
egressrouters                                          network.operator.openshift.io/v1              true         EgressRouter                         [delete deletecollection get list patch create update watch]
operatorpkis                                           network.operator.openshift.io/v1              true         OperatorPKI                          [delete deletecollection get list patch create update watch]
ingressclasses                                         networking.k8s.io/v1                          false        IngressClass                         [create delete deletecollection get list patch update watch]
ingresses                             ing              networking.k8s.io/v1                          true         Ingress                              [create delete deletecollection get list patch update watch]
networkpolicies                       netpol           networking.k8s.io/v1                          true         NetworkPolicy                        [create delete deletecollection get list patch update watch]
runtimeclasses                                         node.k8s.io/v1                                false        RuntimeClass                         [create delete deletecollection get list patch update watch]
oauthaccesstokens                                      oauth.openshift.io/v1                         false        OAuthAccessToken                     [create delete deletecollection get list patch update watch]
oauthauthorizetokens                                   oauth.openshift.io/v1                         false        OAuthAuthorizeToken                  [create delete deletecollection get list patch update watch]
oauthclientauthorizations                              oauth.openshift.io/v1                         false        OAuthClientAuthorization             [create delete deletecollection get list patch update watch]
oauthclients                                           oauth.openshift.io/v1                         false        OAuthClient                          [create delete deletecollection get list patch update watch]
tokenreviews                                           oauth.openshift.io/v1                         false        TokenReview                          [create]
useroauthaccesstokens                                  oauth.openshift.io/v1                         false        UserOAuthAccessToken                 [delete get list watch]
authentications                                        operator.openshift.io/v1                      false        Authentication                       [delete deletecollection get list patch create update watch]
cloudcredentials                                       operator.openshift.io/v1                      false        CloudCredential                      [delete deletecollection get list patch create update watch]
clustercsidrivers                                      operator.openshift.io/v1                      false        ClusterCSIDriver                     [delete deletecollection get list patch create update watch]
configs                                                operator.openshift.io/v1                      false        Config                               [delete deletecollection get list patch create update watch]
consoles                                               operator.openshift.io/v1                      false        Console                              [delete deletecollection get list patch create update watch]
csisnapshotcontrollers                                 operator.openshift.io/v1                      false        CSISnapshotController                [delete deletecollection get list patch create update watch]
dnses                                                  operator.openshift.io/v1                      false        DNS                                  [delete deletecollection get list patch create update watch]
etcds                                                  operator.openshift.io/v1                      false        Etcd                                 [delete deletecollection get list patch create update watch]
imagecontentsourcepolicies                             operator.openshift.io/v1alpha1                false        ImageContentSourcePolicy             [delete deletecollection get list patch create update watch]
ingresscontrollers                                     operator.openshift.io/v1                      true         IngressController                    [delete deletecollection get list patch create update watch]
kubeapiservers                                         operator.openshift.io/v1                      false        KubeAPIServer                        [delete deletecollection get list patch create update watch]
kubecontrollermanagers                                 operator.openshift.io/v1                      false        KubeControllerManager                [delete deletecollection get list patch create update watch]
kubeschedulers                                         operator.openshift.io/v1                      false        KubeScheduler                        [delete deletecollection get list patch create update watch]
kubestorageversionmigrators                            operator.openshift.io/v1                      false        KubeStorageVersionMigrator           [delete deletecollection get list patch create update watch]
networks                                               operator.openshift.io/v1                      false        Network                              [delete deletecollection get list patch create update watch]
openshiftapiservers                                    operator.openshift.io/v1                      false        OpenShiftAPIServer                   [delete deletecollection get list patch create update watch]
openshiftcontrollermanagers                            operator.openshift.io/v1                      false        OpenShiftControllerManager           [delete deletecollection get list patch create update watch]
servicecas                                             operator.openshift.io/v1                      false        ServiceCA                            [delete deletecollection get list patch create update watch]
storages                                               operator.openshift.io/v1                      false        Storage                              [delete deletecollection get list patch create update watch]
catalogsources                        catsrc           operators.coreos.com/v1alpha1                 true         CatalogSource                        [delete deletecollection get list patch create update watch]
clusterserviceversions                csv,csvs         operators.coreos.com/v1alpha1                 true         ClusterServiceVersion                [delete deletecollection get list patch create update watch]
installplans                          ip               operators.coreos.com/v1alpha1                 true         InstallPlan                          [delete deletecollection get list patch create update watch]
operatorconditions                    condition        operators.coreos.com/v2                       true         OperatorCondition                    [delete deletecollection get list patch create update watch]
operatorgroups                        og               operators.coreos.com/v1                       true         OperatorGroup                        [delete deletecollection get list patch create update watch]
operators                                              operators.coreos.com/v1                       false        Operator                             [delete deletecollection get list patch create update watch]
subscriptions                         sub,subs         operators.coreos.com/v1alpha1                 true         Subscription                         [delete deletecollection get list patch create update watch]
packagemanifests                                       packages.operators.coreos.com/v1              true         PackageManifest                      [get list]
poddisruptionbudgets                  pdb              policy/v1                                     true         PodDisruptionBudget                  [create delete deletecollection get list patch update watch]
podsecuritypolicies                   psp              policy/v1beta1                                false        PodSecurityPolicy                    [create delete deletecollection get list patch update watch]
projectrequests                                        project.openshift.io/v1                       false        ProjectRequest                       [create list]
projects                                               project.openshift.io/v1                       false        Project                              [create delete get list patch update watch]
appliedclusterresourcequotas                           quota.openshift.io/v1                         true         AppliedClusterResourceQuota          [get list]
clusterresourcequotas                 clusterquota     quota.openshift.io/v1                         false        ClusterResourceQuota                 [create delete deletecollection get list patch update watch]
clusterrolebindings                                    rbac.authorization.k8s.io/v1                  false        ClusterRoleBinding                   [create delete deletecollection get list patch update watch]
clusterroles                                           rbac.authorization.k8s.io/v1                  false        ClusterRole                          [create delete deletecollection get list patch update watch]
rolebindings                                           rbac.authorization.k8s.io/v1                  true         RoleBinding                          [create delete deletecollection get list patch update watch]
roles                                                  rbac.authorization.k8s.io/v1                  true         Role                                 [create delete deletecollection get list patch update watch]
routes                                                 route.openshift.io/v1                         true         Route                                [create delete deletecollection get list patch update watch]
configs                                                samples.operator.openshift.io/v1              false        Config                               [delete deletecollection get list patch create update watch]
priorityclasses                       pc               scheduling.k8s.io/v1                          false        PriorityClass                        [create delete deletecollection get list patch update watch]
rangeallocations                                       security.internal.openshift.io/v1             false        RangeAllocation                      [delete deletecollection get list patch create update watch]
podsecuritypolicyreviews                               security.openshift.io/v1                      true         PodSecurityPolicyReview              [create]
podsecuritypolicyselfsubjectreviews                    security.openshift.io/v1                      true         PodSecurityPolicySelfSubjectReview   [create]
podsecuritypolicysubjectreviews                        security.openshift.io/v1                      true         PodSecurityPolicySubjectReview       [create]
rangeallocations                                       security.openshift.io/v1                      false        RangeAllocation                      [create delete deletecollection get list patch update watch]
securitycontextconstraints            scc              security.openshift.io/v1                      false        SecurityContextConstraints           [create delete deletecollection get list patch update watch]
volumesnapshotclasses                                  snapshot.storage.k8s.io/v1                    false        VolumeSnapshotClass                  [delete deletecollection get list patch create update watch]
volumesnapshotcontents                                 snapshot.storage.k8s.io/v1                    false        VolumeSnapshotContent                [delete deletecollection get list patch create update watch]
volumesnapshots                                        snapshot.storage.k8s.io/v1                    true         VolumeSnapshot                       [delete deletecollection get list patch create update watch]
csidrivers                                             storage.k8s.io/v1                             false        CSIDriver                            [create delete deletecollection get list patch update watch]
csinodes                                               storage.k8s.io/v1                             false        CSINode                              [create delete deletecollection get list patch update watch]
csistoragecapacities                                   storage.k8s.io/v1beta1                        true         CSIStorageCapacity                   [create delete deletecollection get list patch update watch]
storageclasses                        sc               storage.k8s.io/v1                             false        StorageClass                         [create delete deletecollection get list patch update watch]
volumeattachments                                      storage.k8s.io/v1                             false        VolumeAttachment                     [create delete deletecollection get list patch update watch]
clustertasks                                           tekton.dev/v1beta1                            false        ClusterTask                          [delete deletecollection get list patch create update watch]
conditions                                             tekton.dev/v1alpha1                           true         Condition                            [delete deletecollection get list patch create update watch]
pipelineresources                                      tekton.dev/v1alpha1                           true         PipelineResource                     [delete deletecollection get list patch create update watch]
pipelineruns                          pr,prs           tekton.dev/v1beta1                            true         PipelineRun                          [delete deletecollection get list patch create update watch]
pipelines                                              tekton.dev/v1beta1                            true         Pipeline                             [delete deletecollection get list patch create update watch]
runs                                                   tekton.dev/v1alpha1                           true         Run                                  [delete deletecollection get list patch create update watch]
taskruns                              tr,trs           tekton.dev/v1beta1                            true         TaskRun                              [delete deletecollection get list patch create update watch]
tasks                                                  tekton.dev/v1beta1                            true         Task                                 [delete deletecollection get list patch create update watch]
brokertemplateinstances                                template.openshift.io/v1                      false        BrokerTemplateInstance               [create delete deletecollection get list patch update watch]
processedtemplates                                     template.openshift.io/v1                      true         Template                             [create]
templateinstances                                      template.openshift.io/v1                      true         TemplateInstance                     [create delete deletecollection get list patch update watch]
templates                                              template.openshift.io/v1                      true         Template                             [create delete deletecollection get list patch update watch]
clusterinterceptors                   ci               triggers.tekton.dev/v1alpha1                  false        ClusterInterceptor                   [delete deletecollection get list patch create update watch]
clustertriggerbindings                ctb              triggers.tekton.dev/v1beta1                   false        ClusterTriggerBinding                [delete deletecollection get list patch create update watch]
eventlisteners                        el               triggers.tekton.dev/v1beta1                   true         EventListener                        [delete deletecollection get list patch create update watch]
triggerbindings                       tb               triggers.tekton.dev/v1beta1                   true         TriggerBinding                       [delete deletecollection get list patch create update watch]
triggers                              tri              triggers.tekton.dev/v1beta1                   true         Trigger                              [delete deletecollection get list patch create update watch]
triggertemplates                      tt               triggers.tekton.dev/v1beta1                   true         TriggerTemplate                      [delete deletecollection get list patch create update watch]
profiles                                               tuned.openshift.io/v1                         true         Profile                              [delete deletecollection get list patch create update watch]
tuneds                                                 tuned.openshift.io/v1                         true         Tuned                                [delete deletecollection get list patch create update watch]
groups                                                 user.openshift.io/v1                          false        Group                                [create delete deletecollection get list patch update watch]
identities                                             user.openshift.io/v1                          false        Identity                             [create delete deletecollection get list patch update watch]
useridentitymappings                                   user.openshift.io/v1                          false        UserIdentityMapping                  [create delete get patch update]
users                                                  user.openshift.io/v1                          false        User                                 [create delete deletecollection get list patch update watch]
ippools                                                whereabouts.cni.cncf.io/v1alpha1              true         IPPool                               [delete deletecollection get list patch create update watch]
overlappingrangeipreservations                         whereabouts.cni.cncf.io/v1alpha1              true         OverlappingRangeIPReservation        [delete deletecollection get list patch create update watch]
</pre>

## Checking if you have permission to create pods in all namespaces
```
oc auth can-i create pods --all-namespaces
```

The expected output is

<pre>
(jegan@tektutor.org)$ <b>oc auth can-i create pods --all-namespaces</b>
yes
</pre>

## Checking if you have permission to list deployments in the current project
```
oc auth can-i list deployments.apps
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc auth can-i list deployments.apps</b>
yes
</pre>

## Autoscaling deployment without any conditions
```
oc autoscale deployment openshift-spring --min=2 --max=10
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc autoscale deployment openshift-spring --min=2 --max=10</b>
horizontalpodautoscaler.autoscaling/openshift-spring autoscaled
</pre>
