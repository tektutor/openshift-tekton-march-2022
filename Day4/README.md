# Installing a multi-pod application
```
oc delete project jegan
oc new-project jegan
oc new-app mariadb-ephemeral
oc new-app php~https://github.com/wordpress/wordpress
oc logs -f deploy/wordpress
oc expose svc/wordpress
oc get routes
```

When you click on the wordpress route, it would prompt for certain details
you need to give the database name shown in the terminal when you deployed mariadb

username - root
password - root password shown in the terminal
hostname - mariadb.ocp.svc.cluster.local

The hostname you need to replace it with the hostname you see in Administrator --> Networking --> Services --> mariadb --> Hostname


# Tekton
- Kubernetes native CI/CD Framework
- Available as an operator
- adds many custom resources
     Task
     Pipeline
     TaskRun
     PipelineRun
     Workspace
- comes with custom controllers to manage the above custom resources

# Installing TekTon in OpenShift 

Tekton can be installed either via Webconsole Operators --> OperatorHub, or via CLI.

## What is a Custom Resource in Kubernetes/OpenShift ?
- an API extension mechanism in Kubernetes/OpenShift
- helps you add a new kind of object in your Kubernetes/OpenShift Cluster 
  just like Deployment, ReplicaSet, Pod, etc.,
- a Custom Resource Definition(CRD) defines a Custom Resource(CR)
- once a CR is created using CRD it can be accessed using kubectl or oc commands

## What is an Operator in Kubernetes/OpenShift?
Some references
https://kubernetes.io/docs/concepts/extend-kubernetes/operator/

- an Operator is a custom Kubernetes/OpenShift Controller that waches a CR
- Custom Controllers monitors CR, compares its desired with actual state, 
  if it deviates takes appropriate actions
- Operators helps
  - scaling up/down a Custom Resource
  - upgrading a CR from one version to another
  - engineers & developers who would like to extend Kubernetes API to manage their site 
    and software

## Operator Framework Choices
Charmed Operator Framework - https://juju.is/<br>
kopf - https://github.com/nolar/kopf<br>
Kubebuilder - https://book.kubebuilder.io/<br>
KubeOps - https://buehler.github.io/dotnet-operator-sdk/<br>
KUDO - https://kudo.dev/<br>
MetaController - https://metacontroller.github.io/metacontroller/intro.html<br>
Operator Framework - https://operatorframework.io/<br>
shell operator - https://github.com/flant/shell-operator<br>

## What is Operator SDK?
- builds on top of Kuberenetes controller-runtime libraries
- provides essential Kubernetes controller runtimes in Go programming lanaguage
- a set of tools that helps in developing, building and deploying an Operator into i
  Kubernetes/OpenShift

## What is Operator Lifecycle Manager (OLM) ?
- it takes the Operator pattern one level above by creating a OLM Operator which manages Operators
- it lets you define Operators declaratively

## What is Operator Metering?
- is used to analyze the resource usage of Operators running in Kubernetes/OpenShift
- CPU Usage, memory usage, and other metrics
