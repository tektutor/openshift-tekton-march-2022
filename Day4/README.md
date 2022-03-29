# ⛹️‍♂️ Lab - Installing a multi-pod WordPress with maridb application
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

## Tekton CI/CD Pipeline
- Tekton is an opensource knative application that helps you create CI/CD 
  pipeline within Kubernetes/OpenShift cluster.
- Tekton supports both Kubernetes and OpenShift
- is a set of custom kubernetes resources
- Available as an operator or can be installed via manifest files from CLI
- adds many custom resources
     Task
     Pipeline
     TaskRun
     PipelineRun
     Workspace
- comes with custom controllers to manage the above custom resources

## Installing Tekton within OpenShift Web console

🔴 Only one person can perform this task in a Cluster as Tekton is installed cluster wide. 🔴

From the OpenShift web console, switch to Administrator view.  Navigate to Operators --> OperatorHub Menu on the Left side as shown in the screenshot below. In the search text box, you need to type "OpenShift Pipelines" to narrow down the search
![Operators](OCP_Pipeline_Operator.png)

Select "Red Hat OpenShift Pipelines" and Click on Install button.
![Operators](OCP_Pipeline_Install.png)

You may now click on the Install button
![Operators](OCP_Pipeline_Install2.png)
Once you Click on the Install button, you will be taken to below screen

![Operators](OCP_Install3.png)
Once the Installation is complete, it will take you the below page

![Operators](OCP_View_Operator.png)
You may now click on the View Operator button which then takes you to the final page.

![Operators](OCP_View2.png)
![Operators](OCP_View_Operator3.png)

Congratulations! you have installed Tekton in your OpenShift Cluster.

## Installing Tekton via CLI
```
oc new-project tekton-pipelines
oc adm policy add-scc-to-user anyuid -z tekton-pipelines-controller
oc adm policy add-scc-to-user anyuid -z tekton-pipelines-webhook
oc apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.notags.yaml
```

## ⛹️‍♀️ Lab - Creating your very first Tekton task
Create a file named hello.yml and paste the below content
<pre>
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello
  namespace: tektutor 
spec:
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - "Hello World !"
</pre>

Now try creating the task as shown below
```
oc apply -f hello.yml
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f hello.yml</b>
task.tekton.dev/hello created
</pre>

You can try listing the task
```
tkn task list
oc get tasks
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn task list</b>
NAME    DESCRIPTION   AGE
hello                 13 seconds ago
(jegan@tektutor.org)$ <b>oc get tasks</b>
NAME    AGE
hello   23s
</pre>

You can try to find more details about the task
```
oc describe task/hello
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc describe task/hello</b>
Name:         hello
Namespace:    tektutor
Labels:       <none>
Annotations:  <none>
API Version:  tekton.dev/v1beta1
Kind:         Task
Metadata:
  Creation Timestamp:  2022-03-29T06:59:19Z
  Generation:          1
  Managed Fields:
    API Version:  tekton.dev/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:steps:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2022-03-29T06:59:19Z
  Resource Version:  606975
  UID:               32c0cbd4-f37e-4722-b316-c55f4093c66d
Spec:
  Steps:
    Args:
      Hello World !
    Command:(jegan@tektutor.org)$ <b>oc describe task/hello</b>
Name:         hello
Namespace:    tektutor
Labels:       <none>
Annotations:  <none>
API Version:  tekton.dev/v1beta1
Kind:         Task
Metadata:
  Creation Timestamp:  2022-03-29T06:59:19Z
  Generation:          1
  Managed Fields:
    API Version:  tekton.dev/v1beta1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
      f:spec:
        .:
        f:steps:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2022-03-29T06:59:19Z
  Resource Version:  606975
  UID:               32c0cbd4-f37e-4722-b316-c55f4093c66d
Spec:
  Steps:
    Args:
      Hello World !
    Command:
      echo
    Image:  ubuntu
    Name:   echo
Events:     <none>

      echo
    Image:  ubuntu
    Name:   echo
Events:     <none>
</pre>

## ⛹️‍♀️ Lab - Running the Task

We need to create a TaskRun to run the Task.  
You can achieve this either by writing a TaskRun manifest(yaml) file or via this command
```
tkn task start hello
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn task start hello</b>
TaskRun started: hello-run-bdj8j

In order to track the TaskRun progress run:
tkn taskrun logs hello-run-bdj8j -f -n tektutor
</pre>

You may now check the output of the TaskRun as shown below
```
tkn taskrun logs hello-run-bdj8j -f -n tektutor
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-run-bdj8j -f -n tektutor</b>

[echo] Hello World !
</pre>

## ⛹️ Lab - Passing arguments to a Tekton Task
Let's create a hello-task-with-params.yml file and paste the below content into the file

<pre>
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello-task-with-params
  namespace: tektutor
spec:
  params:
    - name: message
      type: string
      description: this is an optional field that tells tells the use of this message param
      default: Hello TekTon Task !
  steps:
    - name: echo
      image: ubuntu
      command:
        - echo
      args:
        - $(params.message)
</pre>

Create the task
```
oc apply -f hello-task-with-params.yml
```
Expected output 
<pre>
(jegan@tektutor.org)$ <b>oc apply -f hello-task-with-params.yml</b>
task.tekton.dev/hello-task-with-params created
</pre>

You can then list and see if the task is created successfully
```
oc get tasks
tkn task list
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get tasks</b>
NAME                     AGE
hello                    13m
hello-task-with-params   4s
(jegan@tektutor.org)$ <b>tkn task list</b>
NAME                     DESCRIPTION   AGE
hello                                  13 minutes ago
hello-task-with-params                 14 seconds ago
</pre>

Now let's execute the task
```
tkn task start hello-task-with-params
```

Expected output is

<pre>
(jegan@tektutor.org)$ <b>tkn task start hello-task-with-params</b>
? Value for param `message` of type `string`? (Default is `Hello TekTon Task !`) Hello TekTon Task !
TaskRun started: hello-task-with-params-run-xz7c2

In order to track the TaskRun progress run:
tkn taskrun logs hello-task-with-params-run-xz7c2 -f -n tektutor
</pre>

See if the taskrun pods are running/completed
```
oc get pods
oc get po -w
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get po</b>
NAME                                   READY   STATUS            RESTARTS   AGE
hello-run-bdj8j-pod                    0/1     Completed         0          14m
hello-task-with-params-run-xz7c2-pod   0/1     PodInitializing   0          7s
(jegan@tektutor.org)$ <b>oc get po -w</b>
NAME                                   READY   STATUS      RESTARTS   AGE
hello-run-bdj8j-pod                    0/1     Completed   0          14m
hello-task-with-params-run-xz7c2-pod   1/1     Running     0          10s
hello-task-with-params-run-xz7c2-pod   0/1     Completed   0          10s
</pre>

You may check the output of the TaskRun as shown below
```
tkn taskrun logs hello-task-with-params-run-xz7c2 -f -n tektutor
oc logs hello-task-with-params-run-xz7c2-pod
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-task-with-params-run-xz7c2 -f -n tektutor</b>
[echo] Hello TekTon Task !

(jegan@tektutor.org)$ <b>oc logs hello-task-with-params-run-xz7c2-pod</b>
Hello TekTon Task !
</pre>

You may also check the output in the webconsole.


## ⛹️‍♂️ Lab - Tasks with multiple steps
<pre>
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello-task-with-multiple-steps
  namespace: tektutor
spec:
  params:
    - name: msg1
      type: string
      default: Message from Step1
    - name: msg2
      type: string
      default: Message from Step2
  steps:
    - name: step1
      image: ubuntu
      command:
        - echo
      args:
        - $(params.msg1)
    - name: step2
      image: ubuntu
      command:
        - echo
      args:
        - $(params.msg2)
</pre>

Creating a taskrun by dry-running the task
```
tkn task start hello-task-with-multiple-steps --dry-run
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn task start hello-task-with-multiple-steps  --dry-run</b>
? Value for param `msg1` of type `string`? (Default is `Message from Step1`) Message from Step1
? Value for param `msg2` of type `string`? (Default is `Message from Step2`) Message from Step2
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  creationTimestamp: null
  generateName: hello-task-with-multiple-steps-run-
  namespace: tektutor
spec:
  params:
  - name: msg1
    value: Message from Step1
  - name: msg2
    value: Message from Step2
  resources: {}
  serviceAccountName: ""
  taskRef:
    name: hello-task-with-multiple-steps
status:
  podName: ""
</pre>


You may copy/paste the above code in a taskrun yaml file as shown below
<pre>
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  name: hello-task-with-multiple-steps-taskrun
  namespace: tektutor
spec:
  params:
  - name: msg1
    value: Message from Step1
  - name: msg2
    value: Message from Step2
  taskRef:
    name: hello-task-with-multiple-steps
</pre>

You can now execute the taskrun as shown below
```
tkn taskrun start hello-task-with-multiple-steps-run
```
The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f hello-multiple-steps-run.yml</b>
taskrun.tekton.dev/hello-task-with-multiple-steps-run created
</pre>

You can check the status of the taskrun as shown below
```
tkn taskrun list
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun list</b>
NAME                                       STARTED          DURATION     STATUS
<b>hello-task-with-multiple-steps-run         23 seconds ago   16 seconds   Succeeded</b>
hello-python-run-9scjd                     1 hour ago       9 seconds    Succeeded
hello-python-run-2sdd2                     1 hour ago       55 seconds   Succeeded
hello-task-with-multiple-steps-run-cx2pm   4 hours ago      15 seconds   Succeeded
hello-task-with-multiple-steps-run-9h2xs   4 hours ago      35 seconds   Succeeded
hello-task-with-multiple-steps-run-7pn8b   4 hours ago      0 seconds    Failed(CouldntGetTask)
hello-task-with-params-run-gm5kz           4 hours ago      11 seconds   Succeeded
hello-task-with-params-run-xz7c2           4 hours ago      10 seconds   Succeeded
hello-run-bdj8j                            5 hours ago      30 seconds   Succeeded
</pre>

If you wish to check the output, you can try this
```
tkn taskrun logs -f hello-task-with-multiple-steps-run
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs -f hello-task-with-multiple-steps-run</b>
[step1] Message from Step1

[step2] Message from Step2
</pre>

You can also see the logs the recently executed taskrun as shown below
```
tkn taskrun logs --last -f
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs --last -f</b>
[step1] Message from Step1

[step2] Message from Step2
</pre>
