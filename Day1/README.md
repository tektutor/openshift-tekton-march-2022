## You may follow me in medium ( Give me a clap and follow in medium if you found the articles useful )
https://medium.com/tektutor

### My Articles
https://medium.com/tektutor/kubernetes-3-node-cluster-setup-50943378be41

https://medium.com/tektutor/using-metal-lb-on-a-bare-metal-onprem-kubernetes-setup-6d036af1d20c

https://medium.com/tektutor/using-nginx-ingress-controller-in-kubernetes-bare-metal-setup-890eb4e7772

https://medium.com/tektutor/deploying-stateful-applications-in-kubernetes-8ffd46920b55

https://medium.com/tektutor/container-engine-vs-container-runtime-667a99042f3

# Docker
- is an container engine
- is developed by Docker Inc organization using Go Programming Langauage
- it comes in flavours
   1. Community Edition (CE)
   2. Enterprise Edition (EE)
- follows Client/Server Architecture
    client - docker
    server - dockerd ( Linux service/daemon ) - Docker Engine
- depends on containerd container runtime
- containerd depends on 
    runc to create and manage containers in Linux
    hcsshim to create and manage containers in Windows

# Kubernetes
- is an Container Orchestration Tool
    - container to container communication
    - it has inbuilt monitoring using controllers
    - provides an eco system where your application can be deployed and made HA
    - self-healing platform
    - scale up/down
    - rolling update
         - upgrading your application from one version to another without any downtime
         - optionally can rollback if recently deployed version is unstable
- developed by Google in Go Programming Language
- open source
- it supports many different container runtimes
     - Docker ( Default )
     - CRI-O
     - Rkt
- cluster
    - one or more master nodes
        - Control Plane ( Runs in Master Node )
            1. API Server
                 - implements all Orchestration features as REST API
                 - uses etcd key/value datastore to store and maintain cluster state
                 - is only component which will access etcd datastore
                 - all communication within Kubernetes will happen only via API Server
                 - no two components can talk to each other directly
            2. etcd
            3. Scheduler
            4. Controller Managers ( Monitoring & Healing/Repair )
                - Deployment Controller
                - Replicaset Controller
                - Replication Controller
                - Endpoint Controller
                - Statefulset Controller
                - DaemonSet Controller
    - one or more worker nodes


Kubernetes Objects
 - Pod
    - a collection of related containers
    - each Pod has its own network namespace
       i.e it has it own software definied network interface card (NIC)
    - each Pod get its private IP
    - has its file system and shell
    - user-defined application are hosted within one of the container Pod
    - each Pod has atleast two containers
       1. pause container ( common to every Pod ) 
            - this is container that supplies network stack 
            - also provides host name
            - the sole purpose of this container to supply the network stack (7 OSI layers)
            - NIC ( Network Interface Card )
       2. application container
            - this is where user-defined application run
 - Deployment
    - represents a user-defined application
    - indicate how many instances of your application should be hosted in K8s cluster
    - supports rolling update
    - creates something called ReplicaSet
    - each Deployment may have one or more ReplicaSet

- ReplicaSet
    - manages Pod(s)
    - can indicate how many Pod instances should be running at point of time
    - scale up/down


Microservice 
   - frontend ( springboot frontend application )
       - should create a separate deployment for Frontend
   - backend ( mysql db )
       - should create a separate deployment for Backend

  
## Listing nodes in Kubernetes cluster
```
kubectl get nodes
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>kubectl get nodes</b>
NAME                             STATUS   ROLES           AGE   VERSION
master-1.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
master-2.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
master-3.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
worker-1.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35
worker-2.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35
</pre>

## Listing nodes in OpenShift cluster
```
oc get nodes
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get nodes</b>
NAME                             STATUS   ROLES           AGE   VERSION
master-1.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
master-2.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
master-3.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35
worker-1.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35
worker-2.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35
</pre>


## Listing nodes in OpenShift with more details
```
oc get nodes -o wide
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get nodes -o wide</b>
NAME                             STATUS   ROLES           AGE   VERSION           INTERNAL-IP       EXTERNAL-IP   OS-IMAGE                                                       KERNEL-VERSION                 CONTAINER-RUNTIME
master-1.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35   192.168.122.13    <none>        Red Hat Enterprise Linux CoreOS 49.84.202202141503-0 (Ootpa)   4.18.0-305.34.2.el8_4.x86_64   cri-o://1.22.1-16.rhaos4.9.git12fa1c7.el8
master-2.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35   192.168.122.136   <none>        Red Hat Enterprise Linux CoreOS 49.84.202202141503-0 (Ootpa)   4.18.0-305.34.2.el8_4.x86_64   cri-o://1.22.1-16.rhaos4.9.git12fa1c7.el8
master-3.tektutor.tektutor.org   Ready    master,worker   25d   v1.22.3+b93fd35   192.168.122.15    <none>        Red Hat Enterprise Linux CoreOS 49.84.202202141503-0 (Ootpa)   4.18.0-305.34.2.el8_4.x86_64   cri-o://1.22.1-16.rhaos4.9.git12fa1c7.el8
worker-1.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35   192.168.122.69    <none>        Red Hat Enterprise Linux CoreOS 49.84.202202141503-0 (Ootpa)   4.18.0-305.34.2.el8_4.x86_64   cri-o://1.22.1-16.rhaos4.9.git12fa1c7.el8
worker-2.tektutor.tektutor.org   Ready    worker          25d   v1.22.3+b93fd35   192.168.122.143   <none>        Red Hat Enterprise Linux CoreOS 49.84.202202141503-0 (Ootpa)   4.18.0-305.34.2.el8_4.x86_64   cri-o://1.22.1-16.rhaos4.9.git12fa1c7.el8
</pre>

## Listing projects
```
oc get projects
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get projects</b>
NAME                                               DISPLAY NAME   STATUS
default                                                           Active
jegan                                                             Terminating
kube-node-lease                                                   Active
kube-public                                                       Active
kube-system                                                       Active
openshift                                                         Active
openshift-apiserver                                               Active
openshift-apiserver-operator                                      Active
openshift-authentication                                          Active
openshift-authentication-operator                                 Active
openshift-cloud-controller-manager                                Active
openshift-cloud-controller-manager-operator                       Active
openshift-cloud-credential-operator                               Active
openshift-cluster-csi-drivers                                     Active
openshift-cluster-machine-approver                                Active
openshift-cluster-node-tuning-operator                            Active
openshift-cluster-samples-operator                                Active
openshift-cluster-storage-operator                                Active
openshift-cluster-version                                         Active
openshift-config                                                  Active
openshift-config-managed                                          Active
openshift-config-operator                                         Active
openshift-console                                                 Active
openshift-console-operator                                        Active
openshift-console-user-settings                                   Active
openshift-controller-manager                                      Active
openshift-controller-manager-operator                             Active
openshift-dns                                                     Active
openshift-dns-operator                                            Active
openshift-etcd                                                    Active
openshift-etcd-operator                                           Active
openshift-host-network                                            Active
openshift-image-registry                                          Active
openshift-infra                                                   Active
openshift-ingress                                                 Active
openshift-ingress-canary                                          Active
openshift-ingress-operator                                        Active
openshift-insights                                                Active
openshift-kni-infra                                               Active
openshift-kube-apiserver                                          Active
openshift-kube-apiserver-operator                                 Active
openshift-kube-controller-manager                                 Active
openshift-kube-controller-manager-operator                        Active
openshift-kube-scheduler                                          Active
openshift-kube-scheduler-operator                                 Active
openshift-kube-storage-version-migrator                           Active
openshift-kube-storage-version-migrator-operator                  Active
openshift-kubevirt-infra                                          Active
openshift-machine-api                                             Active
openshift-machine-config-operator                                 Active
openshift-marketplace                                             Active
openshift-monitoring                                              Active
openshift-multus                                                  Active
openshift-network-diagnostics                                     Active
openshift-network-operator                                        Active
openshift-node                                                    Active
openshift-oauth-apiserver                                         Active
openshift-openstack-infra                                         Active
openshift-operator-lifecycle-manager                              Active
openshift-operators                                               Active
openshift-ovirt-infra                                             Active
openshift-sdn                                                     Active
openshift-service-ca                                              Active
openshift-service-ca-operator                                     Active
openshift-user-workload-monitoring                                Active
openshift-vsphere-infra                                           Active
tekton-pipelines  
</pre>


## Creating a new project in OpenShift
```
oc new-project <project-name>
```



## Switching a particular project
```
oc project <project-name>
```

## Deleting a project
```
oc delete project <project-name>
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>
