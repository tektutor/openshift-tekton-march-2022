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

  


