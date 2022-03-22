## You may follow me in medium ( Hit the clap button a few times and follow the author in medium )
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

  
## ⛹️‍♀️ Lab - Listing nodes in Kubernetes cluster
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

## ⛹️‍♂️ Lab - Listing nodes in OpenShift cluster
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


## ⛹️ Listing nodes in OpenShift with more details
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

## ⛹️‍♀️ Lab - Listing projects
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


## ⛹️‍♂️ Lab - Creating a new project in OpenShift
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


## ⛹️‍♀️ Lab - Switching a particular project
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

## ⛹️‍♀️ Lab - Deploying an application from source (S2I)
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

## ⛹️ Lab - Checking the status of the application deployment
```
oc logs -f buildconfig/rails-postgresql-example
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc logs -f buildconfig/rails-postgresql-example</b>
Cloning "https://github.com/sclorg/rails-ex.git" ...
	Commit:	7214e7cad3695d51f17a247afcae48cd9ac37efa (Fix s2i assemble failing with Ruby 2.6.)
	Author:	Jarek Prokop <jprokop@redhat.com>
	Date:	Mon Feb 28 15:42:12 2022 +0100
time="2022-03-21T13:38:54Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I0321 13:38:54.335666       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
Caching blobs under "/var/cache/blobs".
Trying to pull image-registry.openshift-image-registry.svc:5000/openshift/ruby@sha256:7e3204d6f79a375c4a8fe6e29b76df426a8c024e3a93d0af02a00ff89d2b10c9...
Getting image source signatures
Copying blob sha256:aad543859364662ddb264ad5752fd9449d47410b9efa0278463c0a9c578b79c6
Copying blob sha256:25da00f4424f2fd3211cf49a8ee67241041bfb2b50fe1f9306e6674a3a2a8f0b
Copying blob sha256:5dcbdc60ea6b60326f98e2b49d6ebcb7771df4b70c6297ddf2d7dede6692df6e
Copying blob sha256:8671113e1c57d3106acaef2383f9bbfe1c45a26eacb03ec82786a494e15956c3
Copying blob sha256:79a56ba04a301eb949644bca29f18b1879b6f305091ef1eb8068a0f5828db863
Copying config sha256:f64fbb25bd2c5d66ce748b338e09ce97497d89bd23676326adb5761ba2625f78
Writing manifest to image destination
Storing signatures
Generating dockerfile with builder image image-registry.openshift-image-registry.svc:5000/openshift/ruby@sha256:7e3204d6f79a375c4a8fe6e29b76df426a8c024e3a93d0af02a00ff89d2b10c9
Replaced Dockerfile FROM image image-registry.openshift-image-registry.svc:5000/openshift/ruby@sha256:7e3204d6f79a375c4a8fe6e29b76df426a8c024e3a93d0af02a00ff89d2b10c9
Adding transient rw bind mount for /run/secrets/rhsm
[1/3] STEP 1/10: FROM image-registry.openshift-image-registry.svc:5000/openshift/ruby@sha256:7e3204d6f79a375c4a8fe6e29b76df426a8c024e3a93d0af02a00ff89d2b10c9 AS appimage7b848b6bf22841f18718de2197d76d49
[1/3] STEP 2/10: LABEL "io.openshift.build.commit.date"="Mon Feb 28 15:42:12 2022 +0100"       "io.openshift.build.commit.id"="7214e7cad3695d51f17a247afcae48cd9ac37efa"       "io.openshift.build.commit.ref"="master"       "io.openshift.build.commit.message"="Fix s2i assemble failing with Ruby 2.6."       "io.openshift.build.source-location"="https://github.com/sclorg/rails-ex.git"       "io.openshift.build.image"="image-registry.openshift-image-registry.svc:5000/openshift/ruby@sha256:7e3204d6f79a375c4a8fe6e29b76df426a8c024e3a93d0af02a00ff89d2b10c9"       "io.openshift.build.commit.author"="Jarek Prokop <jprokop@redhat.com>"
[1/3] STEP 3/10: ENV OPENSHIFT_BUILD_NAME="rails-postgresql-example-1"     OPENSHIFT_BUILD_NAMESPACE="jegan"     OPENSHIFT_BUILD_SOURCE="https://github.com/sclorg/rails-ex.git"     OPENSHIFT_BUILD_COMMIT="7214e7cad3695d51f17a247afcae48cd9ac37efa"     RUBYGEM_MIRROR=""
[1/3] STEP 4/10: USER root
[1/3] STEP 5/10: COPY upload/scripts /tmp/scripts
[1/3] STEP 6/10: COPY upload/src /tmp/src
[1/3] STEP 7/10: RUN chown -R 1001:0 /tmp/scripts /tmp/src
[1/3] STEP 8/10: USER 1001
[1/3] STEP 9/10: RUN /tmp/scripts/assemble
/tmp/src ~
---> Building your Ruby application from source ...
You are replacing the current local value of deployment, which is currently nil
You are replacing the current local value of without, which is currently nil
---> Running 'bundle install --retry 2' ...
You are replacing the current local value of path, which is currently nil
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
Fetching gem metadata from https://rubygems.org/..........
Fetching rake 13.0.3
Installing rake 13.0.3
Fetching concurrent-ruby 1.1.8
Installing concurrent-ruby 1.1.8
Fetching i18n 1.8.10
Installing i18n 1.8.10
Fetching minitest 5.14.4
Installing minitest 5.14.4
Fetching tzinfo 2.0.4
Installing tzinfo 2.0.4
Fetching zeitwerk 2.4.2
Installing zeitwerk 2.4.2
Fetching activesupport 6.1.3.1
Installing activesupport 6.1.3.1
Fetching builder 3.2.4
Installing builder 3.2.4
Fetching erubi 1.10.0
Installing erubi 1.10.0
Fetching mini_portile2 2.5.1
Installing mini_portile2 2.5.1
Fetching racc 1.5.2
Installing racc 1.5.2 with native extensions
Fetching nokogiri 1.11.3 (x86_64-linux)
Installing nokogiri 1.11.3 (x86_64-linux)
Fetching rails-dom-testing 2.0.3
Installing rails-dom-testing 2.0.3
Fetching crass 1.0.6
Installing crass 1.0.6
Fetching loofah 2.9.1
Installing loofah 2.9.1
Fetching rails-html-sanitizer 1.3.0
Installing rails-html-sanitizer 1.3.0
Fetching actionview 6.1.3.1
Installing actionview 6.1.3.1
Fetching rack 2.2.3
Installing rack 2.2.3
Fetching rack-test 1.1.0
Installing rack-test 1.1.0
Fetching actionpack 6.1.3.1
Installing actionpack 6.1.3.1
Fetching nio4r 2.5.7
Installing nio4r 2.5.7 with native extensions
Fetching websocket-extensions 0.1.5
Installing websocket-extensions 0.1.5
Fetching websocket-driver 0.7.3
Installing websocket-driver 0.7.3 with native extensions
Fetching actioncable 6.1.3.1
Installing actioncable 6.1.3.1
Fetching globalid 0.4.2
Installing globalid 0.4.2
Fetching activejob 6.1.3.1
Installing activejob 6.1.3.1
Fetching activemodel 6.1.3.1
Installing activemodel 6.1.3.1
Fetching activerecord 6.1.3.1
Installing activerecord 6.1.3.1
Fetching marcel 1.0.1
Installing marcel 1.0.1
Fetching mini_mime 1.0.3
Installing mini_mime 1.0.3
Fetching activestorage 6.1.3.1
Installing activestorage 6.1.3.1
Fetching mail 2.7.1
Installing mail 2.7.1
Fetching actionmailbox 6.1.3.1
Installing actionmailbox 6.1.3.1
Fetching actionmailer 6.1.3.1
Installing actionmailer 6.1.3.1
Fetching actiontext 6.1.3.1
Installing actiontext 6.1.3.1
Using bundler 1.17.2
Fetching coffee-script-source 1.12.2
Installing coffee-script-source 1.12.2
Fetching execjs 2.7.0
Installing execjs 2.7.0
Fetching coffee-script 2.4.1
Installing coffee-script 2.4.1
Fetching method_source 1.0.0
Installing method_source 1.0.0
Fetching thor 1.1.0
Installing thor 1.1.0
Fetching railties 6.1.3.1
Installing railties 6.1.3.1
Fetching coffee-rails 5.0.0
Installing coffee-rails 5.0.0
Fetching ffi 1.15.0
Installing ffi 1.15.0 with native extensions
Fetching jbuilder 2.11.2
Installing jbuilder 2.11.2
Fetching rb-fsevent 0.10.4
Installing rb-fsevent 0.10.4
Fetching rb-inotify 0.10.1
Installing rb-inotify 0.10.1
Fetching listen 3.5.1
Installing listen 3.5.1
Fetching pg 1.2.3
Installing pg 1.2.3 with native extensions
Fetching puma 5.2.2
Installing puma 5.2.2 with native extensions
Fetching sprockets 4.0.2
Installing sprockets 4.0.2
Fetching sprockets-rails 3.2.2
Installing sprockets-rails 3.2.2
Fetching rails 6.1.3.1
Installing rails 6.1.3.1
Fetching redis 4.2.5
Installing redis 4.2.5
Fetching sassc 2.4.0
Installing sassc 2.4.0 with native extensions
Fetching tilt 2.0.10
Installing tilt 2.0.10
Fetching sassc-rails 2.1.2
Installing sassc-rails 2.1.2
Fetching sass-rails 6.0.0
Installing sass-rails 6.0.0
Fetching sqlite3 1.4.2
Installing sqlite3 1.4.2 with native extensions
Fetching turbolinks-source 5.2.0
Installing turbolinks-source 5.2.0
Fetching turbolinks 5.2.1
Installing turbolinks 5.2.1
Fetching uglifier 4.2.0
Installing uglifier 4.2.0
Bundle complete! 18 Gemfile dependencies, 62 gems now installed.
Gems in the groups development and test were not installed.
Bundled gems are installed into `./bundle`
---> Cleaning up unused ruby gems ...
Running `bundle clean --verbose` with bundler 1.17.2
Frozen, using resolution from the lockfile
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
---> No master key present in environment, generating ...
Adding config/master.key to store the encryption key: 83f92cfb0ca6d6602cc6c2fa8c40568e

Save this in a password manager your team can access.

If you lose the key, no one, including you, can access anything encrypted with it.

      create  config/master.key

File encrypted and saved.
~
---> Installing application source ...
---> Building your Ruby application from source ...
---> Running 'bundle install --retry 2 --deployment --without development:test' ...
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
Using rake 13.0.3
Using concurrent-ruby 1.1.8
Using i18n 1.8.10
Using minitest 5.14.4
Using tzinfo 2.0.4
Using zeitwerk 2.4.2
Using activesupport 6.1.3.1
Using builder 3.2.4
Using erubi 1.10.0
Using mini_portile2 2.5.1
Using racc 1.5.2
Using nokogiri 1.11.3 (x86_64-linux)
Using rails-dom-testing 2.0.3
Using crass 1.0.6
Using loofah 2.9.1
Using rails-html-sanitizer 1.3.0
Using actionview 6.1.3.1
Using rack 2.2.3
Using rack-test 1.1.0
Using actionpack 6.1.3.1
Using nio4r 2.5.7
Using websocket-extensions 0.1.5
Using websocket-driver 0.7.3
Using actioncable 6.1.3.1
Using globalid 0.4.2
Using activejob 6.1.3.1
Using activemodel 6.1.3.1
Using activerecord 6.1.3.1
Using marcel 1.0.1
Using mini_mime 1.0.3
Using activestorage 6.1.3.1
Using mail 2.7.1
Using actionmailbox 6.1.3.1
Using actionmailer 6.1.3.1
Using actiontext 6.1.3.1
Using bundler 1.17.2
Using coffee-script-source 1.12.2
Using execjs 2.7.0
Using coffee-script 2.4.1
Using method_source 1.0.0
Using thor 1.1.0
Using railties 6.1.3.1
Using coffee-rails 5.0.0
Using ffi 1.15.0
Using jbuilder 2.11.2
Using rb-fsevent 0.10.4
Using rb-inotify 0.10.1
Using listen 3.5.1
Using pg 1.2.3
Using puma 5.2.2
Using sprockets 4.0.2
Using sprockets-rails 3.2.2
Using rails 6.1.3.1
Using redis 4.2.5
Using sassc 2.4.0
Using tilt 2.0.10
Using sassc-rails 2.1.2
Using sass-rails 6.0.0
Using sqlite3 1.4.2
Using turbolinks-source 5.2.0
Using turbolinks 5.2.1
Using uglifier 4.2.0
Bundle complete! 18 Gemfile dependencies, 62 gems now installed.
Gems in the groups development and test were not installed.
Bundled gems are installed into `./bundle`
---> Cleaning up unused ruby gems ...
Running `bundle clean --verbose` with bundler 1.17.2
Frozen, using resolution from the lockfile
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x86-mswin32, x64-mingw32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java`.
---> Starting asset compilation ...
I, [2022-03-21T13:42:47.586096 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/manifest-dad05bf766af0fe3d79dd746db3c1361c0583026cdf35d6a2921bccaea835331.js
I, [2022-03-21T13:42:47.586847 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/manifest-dad05bf766af0fe3d79dd746db3c1361c0583026cdf35d6a2921bccaea835331.js.gz
I, [2022-03-21T13:42:47.587831 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/application-6867c76f0b1d4cfaaed1eca5a99a7c9cf930ed33669a918fec1f63bec3bb2c30.js
I, [2022-03-21T13:42:47.588599 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/application-6867c76f0b1d4cfaaed1eca5a99a7c9cf930ed33669a918fec1f63bec3bb2c30.js.gz
I, [2022-03-21T13:42:47.589327 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/articles-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js
I, [2022-03-21T13:42:47.589976 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/articles-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js.gz
I, [2022-03-21T13:42:47.590366 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/cable-4a60fefcb48f6ca6f45704415249958dd0b022103e785061d603107d88509a5e.js
I, [2022-03-21T13:42:47.591681 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/cable-4a60fefcb48f6ca6f45704415249958dd0b022103e785061d603107d88509a5e.js.gz
I, [2022-03-21T13:42:47.592032 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/comments-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js
I, [2022-03-21T13:42:47.592540 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/comments-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js.gz
I, [2022-03-21T13:42:47.593068 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/welcome-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js
I, [2022-03-21T13:42:47.593532 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/welcome-27cfb9694c5e92d25d972c2b4a2d2e222ad088aef866823f772241c1db423402.js.gz
I, [2022-03-21T13:42:47.594327 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/application-30e2c46de053e7de3812037627d684e34d284d750d562538351e4629c5244306.css
I, [2022-03-21T13:42:47.596354 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/application-30e2c46de053e7de3812037627d684e34d284d750d562538351e4629c5244306.css.gz
I, [2022-03-21T13:42:47.599006 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/articles-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css
I, [2022-03-21T13:42:47.600033 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/articles-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css.gz
I, [2022-03-21T13:42:47.600312 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/comments-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css
I, [2022-03-21T13:42:47.600508 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/comments-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css.gz
I, [2022-03-21T13:42:47.600732 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/scaffolds-30e2c46de053e7de3812037627d684e34d284d750d562538351e4629c5244306.css
I, [2022-03-21T13:42:47.600893 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/scaffolds-30e2c46de053e7de3812037627d684e34d284d750d562538351e4629c5244306.css.gz
I, [2022-03-21T13:42:47.601117 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/welcome-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css
I, [2022-03-21T13:42:47.601276 #1026]  INFO -- : Writing /opt/app-root/src/public/assets/welcome-04024382391bb910584145d8113cf35ef376b55d125bb4516cebeb14ce788597.css.gz
[1/3] STEP 10/10: CMD /usr/libexec/s2i/run
time="2022-03-21T13:42:48Z" level=warning msg="Adding metacopy option, configured globally"
Getting image source signatures
Copying blob sha256:a9820c2af00a34f160836f6ef2044d88e6019ca19b3c15ec22f34afe9d73f41c
Copying blob sha256:3d5ecee9360ea8711f32d2af0cab1eae4d53140496f961ca1a634b5e2e817412
Copying blob sha256:b3c6eff0b4bd8d4086050b0261648dbf437c9cdc68b9459e6c30116873e798ec
Copying blob sha256:68c30199e97234de679a7a7bae4eaff19744b913ea2b3500461305312fefdd3c
Copying blob sha256:59cc7d75cb41476fc0975c5b4bbaa8b695db1a2db55bd1be9592e0f72857f8ba
Copying blob sha256:e0c26c434178344cee4b9928577903782bcb664823c8475b67d1601fcaf20265
Copying config sha256:f0c963f068699f32de844a38994526892d5ea1e0822e731e7d01b897548fde4e
Writing manifest to image destination
Storing signatures
--> f0c963f0686
[2/3] STEP 1/2: FROM f0c963f068699f32de844a38994526892d5ea1e0822e731e7d01b897548fde4e
[2/3] STEP 2/2: RUN /bin/sh -ic 'bundle exec rake test'
sh: cannot set terminal process group (-1): Inappropriate ioctl for device
sh: no job control in this shell
Run options: --seed 46629

Running: Finished in 0.172341s, 5.8025 runs/s, 5.8025 assertions/s.
1 runs, 1 assertions, 0 failures, 0 errors, 0 skips
[3/3] STEP 1/1: FROM f0c963f068699f32de844a38994526892d5ea1e0822e731e7d01b897548fde4e
[3/3] COMMIT temp.builder.openshift.io/jegan/rails-postgresql-example-1:d7616cc9
--> f0c963f0686
Successfully tagged temp.builder.openshift.io/jegan/rails-postgresql-example-1:d7616cc9
f0c963f068699f32de844a38994526892d5ea1e0822e731e7d01b897548fde4e

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/rails-postgresql-example:latest ...
Getting image source signatures
Copying blob sha256:5dcbdc60ea6b60326f98e2b49d6ebcb7771df4b70c6297ddf2d7dede6692df6e
Copying blob sha256:aad543859364662ddb264ad5752fd9449d47410b9efa0278463c0a9c578b79c6
Copying blob sha256:8671113e1c57d3106acaef2383f9bbfe1c45a26eacb03ec82786a494e15956c3
Copying blob sha256:25da00f4424f2fd3211cf49a8ee67241041bfb2b50fe1f9306e6674a3a2a8f0b
Copying blob sha256:79a56ba04a301eb949644bca29f18b1879b6f305091ef1eb8068a0f5828db863
Copying blob sha256:e0c26c434178344cee4b9928577903782bcb664823c8475b67d1601fcaf20265
Copying config sha256:f0c963f068699f32de844a38994526892d5ea1e0822e731e7d01b897548fde4e
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/rails-postgresql-example@sha256:023d47657427f68b7046dfb536ee5f1b93f2e2537bb39f0526be2e3a52227017
Push successful
</pre>

## ⛹️‍♀️ Lab - List the deployment configs
```
oc get dc
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get dc</b>
NAME                       REVISION   DESIRED   CURRENT   TRIGGERED BY
postgresql                 1          1         1         config,image(postgresql:12-el8)
rails-postgresql-example   1          1         1         config,image(rails-postgresql-example:latest)
</pre>

## ⛹️‍♂️ Lab - Listing the pods
```
oc get pods
oc get po
oc get po
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get po</b>
NAME                                  READY   STATUS      RESTARTS   AGE
postgresql-1-deploy                   0/1     Completed   0          8m16s
postgresql-1-txp9n                    1/1     Running     0          8m
rails-postgresql-example-1-build      0/1     Completed   0          8m17s
rails-postgresql-example-1-cgr64      1/1     Running     0          3m21s
rails-postgresql-example-1-deploy     0/1     Completed   0          3m35s
rails-postgresql-example-1-hook-pre   0/1     Completed   0          3m32s
</pre>

## ⛹️ Lab - Listing the routes that gives us public URL to access the web page
```
oc get routes
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get routes</b>
NAME                       HOST/PORT                                                   PATH   SERVICES                   PORT    TERMINATION   WILDCARD
rails-postgresql-example   rails-postgresql-example-jegan.apps.tektutor.tektutor.org          rails-postgresql-example   <all>                 None
</pre>

## ⛹️‍♀️ Lab - Accessing the application using Chrome Web browser
```
rails-postgresql-example-jegan.apps.tektutor.tektutor.org 
```

## ⛹️ Lab - Deleting a project
```
oc delete project <project-name>
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc delete project jegan</b>
project.project.openshift.io "jegan" deleted
</pre>

# Different types of Services supported by Kubernetes/OpenShift
1. ClusterIP Service 
2. NodePort Service
3. LoadBalancer Service

## ClusterIP Service
 - internal service
 - for example: db services 
 - they are not accessible outside OpenShift cluster

## NodePort Service
 - external service
 - Kubernetes/OpenShift reserves ports in range 30000 - 32767 for NodePort service on every node in K8s cluster
 - this is accessible from outside the OpenShift cluster also

## LoadBalancer Service
 - external service
 - generally used in Cloud environments like AWS, GCP, Azure, etc.,
 - in AWS this will create a External Network Load Balancer with a public IP
 - the AWS Network Load Balancer will load balance the application Pods
 - this won't work out of the box in on-prem(bare metal) Kubernetes cluster
 
## Create a application deploy using Dockerfile from GitHub
Make sure you are using the correct project
```
oc project
```
Once you are sure, you are in correct project namespace.

create a application deployment using Dockerfile from GitHub repo
```
oc new-app https://github.com/tektutor/spring-ms.git
```
The expected output is
<pre>
(jegan@tektutor.org)$ oc new-app https://github.com/tektutor/spring-ms.git
--> Found container image 1ffbb31 (2 days old) from docker.io for "docker.io/openjdk:latest"

    * An image stream tag will be created as "openjdk:latest" that will track the source image
    * A Docker build using source code from https://github.com/tektutor/spring-ms.git will be created
      * The resulting image will be pushed to image stream tag "spring-ms:latest"
      * Every time "openjdk:latest" changes a new build will be triggered

--> Creating resources ...
    buildconfig.build.openshift.io "spring-ms" created
    deployment.apps "spring-ms" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/spring-ms' to track its progress.
    Run 'oc status' to view your app.
</pre>

You can check the current progress of your application deployment as shown below
```
oc logs -f buildconfig/spring-ms
```
The expected output is
<pre>
(jegan@tektutor.org)$ oc new-app https://github.com/tektutor/spring-ms.git
--> Found container image 1ffbb31 (2 days old) from docker.io for "docker.io/openjdk:latest"

    * An image stream tag will be created as "openjdk:latest" that will track the source image
    * A Docker build using source code from https://github.com/tektutor/spring-ms.git will be created
      * The resulting image will be pushed to image stream tag "spring-ms:latest"
      * Every time "openjdk:latest" changes a new build will be triggered

--> Creating resources ...
    buildconfig.build.openshift.io "spring-ms" created
    deployment.apps "spring-ms" created
--> Success
    Build scheduled, use 'oc logs -f buildconfig/spring-ms' to track its progress.
    Run 'oc status' to view your app.
(jegan@tektutor.org)$ oc logs -f buildconfig/spring-ms
Cloning "https://github.com/tektutor/spring-ms.git" ...
	Commit:	dd9d75b76029ced3481833315d937cfc6cf3975a (Update pom.xml)
	Author:	Jeganathan Swaminathan <jegan@tektutor.org>
	Date:	Tue Mar 1 12:51:21 2022 +0530
Replaced Dockerfile FROM image docker.io/openjdk:latest
time="2022-03-22T07:10:24Z" level=info msg="Not using native diff for overlay, this may cause degraded performance for building images: kernel has CONFIG_OVERLAY_FS_REDIRECT_DIR enabled"
I0322 07:10:24.533728       1 defaults.go:102] Defaulting to storage driver "overlay" with options [mountopt=metacopy=on].
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
time="2022-03-22T07:10:49Z" level=warning msg="Adding metacopy option, configured globally"
--> d52ee780449
STEP 3/5: ENTRYPOINT ["java","-jar","/app.jar"]
--> 77d09f70f47
STEP 4/5: ENV "OPENSHIFT_BUILD_NAME"="spring-ms-1" "OPENSHIFT_BUILD_NAMESPACE"="jegan" "OPENSHIFT_BUILD_SOURCE"="https://github.com/tektutor/spring-ms.git" "OPENSHIFT_BUILD_COMMIT"="dd9d75b76029ced3481833315d937cfc6cf3975a"
--> c5887914e43
STEP 5/5: LABEL "io.openshift.build.commit.author"="Jeganathan Swaminathan <jegan@tektutor.org>" "io.openshift.build.commit.date"="Tue Mar 1 12:51:21 2022 +0530" "io.openshift.build.commit.id"="dd9d75b76029ced3481833315d937cfc6cf3975a" "io.openshift.build.commit.message"="Update pom.xml" "io.openshift.build.commit.ref"="master" "io.openshift.build.name"="spring-ms-1" "io.openshift.build.namespace"="jegan" "io.openshift.build.source-location"="https://github.com/tektutor/spring-ms.git"
COMMIT temp.builder.openshift.io/jegan/spring-ms-1:f6161b76
--> c2843161a55
Successfully tagged temp.builder.openshift.io/jegan/spring-ms-1:f6161b76
c2843161a5573d706f876a3c94183290585144c554cdfa665b9f592b75d38bc3

Pushing image image-registry.openshift-image-registry.svc:5000/jegan/spring-ms:latest ...
Getting image source signatures
Copying blob sha256:7db19695be7be96a8d27e754c74310c6fafa295ecdccbc10e8c5fe5b23854c2a
Copying blob sha256:1824cb7e97fbcfc5c6ffea667f8e19cee765d015fef1b8ad70f96252d9713c95
Copying blob sha256:fc6e4d76e00b859a131e9b4131d92efca34af2da17d3bdc6d5323d473c1972eb
Copying blob sha256:325731a46050609f9598a31fcb18555b9afdfb6ac18a0b89eeceec9495132687
Copying config sha256:c2843161a5573d706f876a3c94183290585144c554cdfa665b9f592b75d38bc3
Writing manifest to image destination
Storing signatures
Successfully pushed image-registry.openshift-image-registry.svc:5000/jegan/spring-ms@sha256:4da645f8029d0a614e4432ba2a31293dddc5423fc7bccfc1d5179a46e641282c
Push successful
</pre>

You can check the deployment
```
oc get deployment
```

The expected output is
<pre>
(jegan@tektutor.org)$ oc get deployment
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
spring-ms   1/1     1            1           105s
</pre>

You can check the replicasets
```
oc get rs
```
The expected output is
<pre>
(jegan@tektutor.org)$ oc get rs
NAME                   DESIRED   CURRENT   READY   AGE
spring-ms-597bc655dd   0         0         0       2m43s
spring-ms-77f44b65fb   1         1         1       2m5s
spring-ms-7b64fff4fc   0         0         0       2m43s
</pre>

You can check the pods
```
oc get po
```
The expected output is
<pre>
(jegan@tektutor.org)$ oc get po
NAME                         READY   STATUS      RESTARTS   AGE
spring-ms-1-build            0/1     Completed   0          2m45s
spring-ms-77f44b65fb-6hch4   1/1     Running     0          2m7s
</pre>
