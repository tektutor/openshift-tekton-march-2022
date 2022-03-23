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
openshift-ingress-canary                           ingress-canary-hl25p                                         1/1     Running     4                 27d
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
