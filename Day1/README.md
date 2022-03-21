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
                 - uses etcd key/value datastore to store and m(jegan@tektutor.org)$ oc new-project jegan
Already on project "jegan" on server "https://api.tektutor.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname

aintain cluster state
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
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-project jegan</b>
Already on project "jegan" on server "https://api.tektutor.tektutor.org:6443".

You can add applications to this project with the 'new-app' command. For example, try:

    oc new-app rails-postgresql-example

to build a new example application in Ruby. Or use kubectl to deploy a simple Kubernetes application:

    kubectl create deployment hello-node --image=k8s.gcr.io/serve_hostname
</pre>


## Switching a particular project
```
oc project <project-name>
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc project default</b>
Now using project "default" on server "https://api.tektutor.tektutor.org:6443".
(jegan@tektutor.org)$ <b>oc project jegan</b>
Now using project "jegan" on server "https://api.tektutor.tektutor.org:6443".
</pre>

## Deploying an application from source (S2I)
```
oc new-app rails-postgresql-example
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc new-app rails-postgresql-example</b>
--> Deploying template "openshift/rails-postgresql-example" to project jegan

     Rails + PostgreSQL (Ephemeral)
     ---------
     An example Rails application with a PostgreSQL database. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/rails-ex/blob/master/README.md.
     
     WARNING: Any data stored will be lost upon pod destruction. Only use this template for testing.

     The following service(s) have been created in your project: rails-postgresql-example, postgresql.
     
     For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/rails-ex/blob/master/README.md.

     * With parameters:
        * Name=rails-postgresql-example
        * Namespace=openshift
        * Memory Limit=512Mi
        * Memory Limit (PostgreSQL)=512Mi
        * Git Repository URL=https://github.com/sclorg/rails-ex.git
        * Git Reference=
        * Context Directory=
        * Application Hostname=
        * GitHub Webhook Secret=ThJWvcJdv1wpHxynUXtsH7UEAQ4coOrA2saQSakD # generated
        * Secret Key=k7u1ej8catllvkeskidfxnhsx4f2ef5fve1x6xvaogmwx7q3nvadis6qrv0npsba0uwpebd31qbykgup1on47nek6key7u7iihrarstjcjrr3m66n7o43a20s8frmml # generated
        * Application Username=openshift
        * Application Password=secret
        * Rails Environment=production
        * Database Service Name=postgresql
        * Database Username=userN7N # generated
        * Database Password=HEQnxWWo # generated
        * Database Name=root
        * Maximum Database Connections=100
        * Shared Buffer Amount=12MB
        * Custom RubyGems Mirror URL=

--> Creating resources ...
    secret "rails-postgresql-example" created
    service "rails-postgresql-example" created
    route.route.openshift.io "rails-postgresql-example" created
    imagestream.image.openshift.io "rails-postgresql-example" created
    buildconfig.build.openshift.io "rails-postgresql-example" created
    deploymentconfig.apps.openshift.io "rails-postgresql-example" created
    service "postgresql" created
    deploymentconfig.apps.openshift.io "postgresql" created
--> Success
    Access your application via route 'rails-postgresql-example-jegan.apps.tektutor.tektutor.org' 
    Build scheduled, use 'oc logs -f buildconfig/rails-postgresql-example' to track its progress.
    Run 'oc status' to view your app.
    </pre>

## Checking the status of the application deployment
```
oc logs -f buildconfig/rails-postgresql-example
```
The expected output is
<pre>

</pre>

## Deleting a project
```
oc delete project <project-name>
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>
