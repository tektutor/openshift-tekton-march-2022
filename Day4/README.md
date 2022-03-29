# ⛹️‍♂️ Lab - Installing a multi-pod WordPress with maridb application

List the templates in openshiftuserG37
```
oc get templates -n openshift
```
Expected output is
<pre>
(jegan@tektutor.org)$ oc get templates -n openshift
NAME                                            DESCRIPTION                                                                        PARAMETERS        OBJECTS
3scale-gateway                                  3scale's APIcast is an NGINX based API gateway used to integrate your interna...   17 (8 blank)      3
amq63-basic                                     Application template for JBoss A-MQ brokers. These can be deployed as standal...   11 (4 blank)      6
amq63-persistent                                An example JBoss A-MQ application. For more information about using this temp...   13 (4 blank)      8
amq63-persistent-ssl                            An example JBoss A-MQ application. For more information about using this temp...   18 (6 blank)      12
amq63-ssl                                       An example JBoss A-MQ application. For more information about using this temp...   16 (6 blank)      10
apicurito                                       Design beautiful, functional APIs with zero coding, using a visual designer f...   7 (1 blank)       7
cache-service                                   Red Hat Data Grid is an in-memory, distributed key/value store.                    8 (1 blank)       4
cakephp-mysql-example                           An example CakePHP application with a MySQL database. For more information ab...   21 (4 blank)      8
cakephp-mysql-persistent                        An example CakePHP application with a MySQL database. For more information ab...   22 (4 blank)      9
dancer-mysql-example                            An example Dancer application with a MySQL database. For more information abo...   18 (5 blank)      8
dancer-mysql-persistent                         An example Dancer application with a MySQL database. For more information abo...   19 (5 blank)      9
datagrid-service                                Red Hat Data Grid is an in-memory, distributed key/value store.                    7 (1 blank)       4
datavirt64-basic-s2i                            Application template for JBoss Data Virtualization 6.4 services built using S2I.   20 (6 blank)      6
datavirt64-extensions-support-s2i               An example JBoss Data Virtualization application. For more information about...    35 (9 blank)      10
datavirt64-ldap-s2i                             Application template for JBoss Data Virtualization 6.4 services that configur...   21 (6 blank)      6oc describe template/mariadb-ephemeral -n openshift
datavirt64-secure-s2i                           An example JBoss Data Virtualization application. For more information about...    51 (22 blank)     8
decisionserver64-amq-s2i                        An example BRMS decision server A-MQ application. For more information about...    30 (5 blank)      10
decisionserver64-basic-s2i                      Application template for Red Hat JBoss BRMS 6.4 decision server applications...    17 (5 blank)      5
django-psql-example                             An example Django application with a PostgreSQL database. For more informatio...   19 (5 blank)      8
django-psql-persistent                          An example Django application with a PostgreSQL database. For more informatio...   20 (5 blank)      9
eap-xp3-basic-s2i                               Example of an application based on JBoss EAP XP. For more information about u...   20 (5 blank)      8
eap74-basic-s2i                                 An example JBoss Enterprise Application Platform application. For more inform...   20 (5 blank)      8
eap74-https-s2i                                 An example JBoss Enterprise Application Platform application configured with...    30 (11 blank)     10
eap74-sso-s2i                                   An example JBoss Enterprise Application Platform application Single Sign-On a...   50 (21 blank)     10
fuse710-console                                 The Red Hat Fuse Console eases the discovery and management of Fuse applicati...   8 (1 blank)       5
httpd-example                                   An example Apache HTTP Server (httpd) application that serves static content....   9 (3 blank)       5
jenkins-ephemeral                               Jenkins service, without persistent storage....                                    11 (all set)      7
jenkins-ephemeral-monitored                     Jenkins service, without persistent storage. ...                                   12 (all set)      8
jenkins-persistent                              Jenkins service, with persistent storage....                                       13 (all set)      8
jenkins-persistent-monitored                    Jenkins service, with persistent storage. ...                                      14 (all set)      9
jws31-tomcat7-basic-s2i                         Application template for JWS applications built using S2I.                         12 (3 blank)      5
jws31-tomcat7-https-s2i                         An example JBoss Web Server application configured for use with https. For mo...   17 (5 blank)      7
jws31-tomcat8-basic-s2i                         An example JBoss Web Server application. For more information about using thi...   12 (3 blank)      5
jws31-tomcat8-https-s2i                         An example JBoss Web Server application. For more information about using thi...   17 (5 blank)      7
jws56-openjdk11-tomcat9-ubi8-basic-s2i          An example JBoss Web Server application. For more information about using thi...   10 (3 blank)      5
jws56-openjdk11-tomcat9-ubi8-https-s2i          An example JBoss Web Server application. For more information about using thi...   15 (5 blank)      7
jws56-openjdk8-tomcat9-ubi8-basic-s2i           An example JBoss Web Server application. For more information about using thi...   10 (3 blank)      5
jws56-openjdk8-tomcat9-ubi8-https-s2i           An example JBoss Web Server application. For more information about using thi...   15 (5 blank)      7
<b>mariadb-ephemeral                               MariaDB database service, without persistent storage. For more information ab...   8 (3 generated)   3</b>
mariadb-persistent                              MariaDB database service, with persistent storage. For more information about...   9 (3 generated)   4
mysql-ephemeral                                 MySQL database service, without persistent storage. For more information abou...   8 (3 generated)   3
mysql-persistent                                MySQL database service, with persistent storage. For more information about u...   9 (3 generated)   4
nginx-example                                   An example Nginx HTTP server and a reverse proxy (nginx) application that ser...   10 (3 blank)      5
nodejs-postgresql-example                       An example Node.js application with a PostgreSQL database. For more informati...   18 (4 blank)      8
nodejs-postgresql-persistent                    An example Node.js application with a PostgreSQL database. For more informati...   19 (4 blank)      9
openjdk-web-basic-s2i                           An example Java application using OpenJDK. For more information about using t...   9 (1 blank)       5
postgresql-ephemeral                            PostgreSQL database service, without persistent storage. For more information...   7 (2 generated)   3
postgresql-persistent                           PostgreSQL database service, with persistent storage. For more information ab...   8 (2 generated)   4
processserver64-amq-mysql-persistent-s2i        An example BPM Suite application with A-MQ and a MySQL database. For more inf...   49 (13 blank)     14
processserver64-amq-mysql-s2i                   An example BPM Suite application with A-MQ and a MySQL database. For more inf...   47 (13 blank)     12
processserver64-amq-postgresql-persistent-s2i   An example BPM Suite application with A-MQ and a PostgreSQL database. For mor...   46 (10 blank)     14
processserver64-amq-postgresql-s2i              An example BPM Suite application with A-MQ and a PostgreSQL database. For mor...   44 (10 blank)     12
processserver64-basic-s2i                       An example BPM Suite application. For more information about using this templ...   17 (5 blank)      5
processserver64-externaldb-s2i                  An example BPM Suite application with a external database. For more informati...   47 (22 blank)     7
processserver64-mysql-persistent-s2i            An example BPM Suite application with a MySQL database. For more information...    40 (14 blank)     10
processserver64-mysql-s2i                       An example BPM Suite application with a MySQL database. For more information...    39 (14 blank)     9
processserver64-postgresql-persistent-s2i       An example BPM Suite application with a PostgreSQL database. For more informa...   37 (11 blank)     10
rails-pgsql-persistent                          An example Rails application with a PostgreSQL database. For more information...   21 (4 blank)      9
rails-postgresql-example                        An example Rails application with a PostgreSQL database. For more information...   20 (4 blank)      8
redis-ephemeral                                 Redis in-memory data structure store, without persistent storage. For more in...   5 (1 generated)   3
redis-persistent                                Redis in-memory data structure store, with persistent storage. For more infor...   6 (1 generated)   4
rhdm711-authoring                               Application template for a non-HA persistent authoring environment, for Red H...   76 (46 blank)     11
rhdm711-authoring-ha                            Application template for a HA persistent authoring environment, for Red Hat D...   92 (47 blank)     17
rhdm711-kieserver                               Application template for a managed KIE Server, for Red Hat Decision Manager 7...   61 (42 blank)     6
rhdm711-prod-immutable-kieserver                Application template for an immutable KIE Server in a production environment,...   66 (45 blank)     8
rhdm711-prod-immutable-kieserver-amq            Application template for an immutable KIE Server in a production environment...    80 (54 blank)     20
rhdm711-trial-ephemeral                         Application template for an ephemeral authoring and testing environment, for...    63 (40 blank)     8
rhpam711-authoring                              Application template for a non-HA persistent authoring environment, for Red H...   80 (46 blank)     12
rhpam711-authoring-ha                           Application template for a HA persistent authoring environment, for Red Hat P...   101 (47 blank)    20
rhpam711-kieserver-externaldb                   Application template for a managed KIE Server with an external database, for...    83 (59 blank)     8
rhpam711-kieserver-mysql                        Application template for a managed KIE Server with a MySQL database, for Red...    70 (42 blank)     9
rhpam711-kieserver-postgresql                   Application template for a managed KIE Server with a PostgreSQL database, for...   71 (42 blank)     9
rhpam711-managed                                Application template for a managed HA production runtime environment, for Red...   87 (46 blank)     14
rhpam711-prod                                   Application template for a managed HA production runtime environment, for Red...   102 (55 blank)    28
rhpam711-prod-immutable-kieserver               Application template for an immutable KIE Server in a production environment,...   76 (45 blank)     11
rhpam711-prod-immutable-kieserver-amq           Application template for an immutable KIE Server in a production environment...    97 (58 blank)     23
rhpam711-prod-immutable-monitor                 Application template for a router and monitoring console in a production envi...   66 (44 blank)     14
rhpam711-trial-ephemeral                        Application template for an ephemeral authoring and testing environment, for...    63 (40 blank)     8
s2i-fuse710-spring-boot-2-camel                 Spring Boot 2 and Camel QuickStart. This example demonstrates how you can use...   18 (3 blank)      3
s2i-fuse710-spring-boot-2-camel-rest-3scale     Spring Boot 2, Camel REST DSL and 3Scale QuickStart. This example demonstrate...   19 (3 blank)      5
s2i-fuse710-spring-boot-2-camel-xml             Spring Boot 2 and Camel Xml QuickStart. This example demonstrates how you can...   18 (3 blank)      3
sso72-https                                     An example RH-SSO 7 application. For more information about using this templa...   26 (15 blank)     6
sso72-mysql                                     An example RH-SSO 7 application with a MySQL database. For more information a...   36 (20 blank)     8
sso72-mysql-persistent                          An example RH-SSO 7 application with a MySQL database. For more information a...   37 (20 blank)     9
sso72-postgresql                                An example RH-SSO 7 application with a PostgreSQL database. For more informat...   33 (17 blank)     8
sso72-postgresql-persistent                     An example RH-SSO 7 application with a PostgreSQL database. For more informat...   34 (17 blank)     9
sso73-https                                     An example application based on RH-SSO 7.3 image. For more information about...    27 (16 blank)     6
sso73-mysql                                     An example application based on RH-SSO 7.3 image. For more information about...    37 (21 blank)     8
sso73-mysql-persistent                          An example application based on RH-SSO 7.3 image. For more information about...    38 (21 blank)     9
sso73-ocp4-x509-https                           An example application based on RH-SSO 7.3 image. For more information about...    13 (7 blank)      5
sso73-ocp4-x509-mysql-persistent                An example application based on RH-SSO 7.3 image. For more information about...    24 (12 blank)     8
sso73-ocp4-x509-postgresql-persistent           An example application based on RH-SSO 7.3 image. For more information about...    21 (9 blank)      8
sso73-postgresql                                An example application based on RH-SSO 7.3 image. For more information about...    34 (18 blank)     8
sso73-postgresql-persistent                     An example application based on RH-SSO 7.3 image. For more information about...    35 (18 blank)     9
sso74-https                                     An example application based on RH-SSO 7.4 on OpenJDK image. For more informa...   27 (16 blank)     6
sso74-ocp4-x509-https                           An example application based on RH-SSO 7.4 on OpenJDK image. For more informa...   13 (7 blank)      5
sso74-ocp4-x509-postgresql-persistent           An example application based on RH-SSO 7.4 on OpenJDK image. For more informa...   21 (9 blank)      8
sso74-postgresql                                An example application based on RH-SSO 7.4 on OpenJDK image. For more informa...   34 (18 blank)     8
sso74-postgresql-persistent                     An example application based on RH-SSO 7.4 on OpenJDK image. For more informa...   35 (18 blank)     9
sso75-https                                     An example application based on RH-SSO 7.5 on OpenJDK image. For more informa...   27 (16 blank)     6
sso75-ocp4-x509-https                           An example application based on RH-SSO 7.5 on OpenJDK image. For more informa...   13 (7 blank)      5
sso75-ocp4-x509-postgresql-persistent           An example application based on RH-SSO 7.5 on OpenJDK image. For more informa...   21 (9 blank)      8
sso75-postgresql                                An example application based on RH-SSO 7.5 on OpenJDK image. For more informa...   34 (18 blank)     8
sso75-postgresql-persistent                     An example applicatiooc describe template/mariadb-ephemeral -n openshiftn based on RH-SSO 7.5 on OpenJDK image. For more informa...   35 (18 blank)     9
</pre>

If you would like to understand little more details of the mariadb-ephemeral template, you may do as shown below
```
oc describe template/mariadb-ephemeral -n openshift
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>oc describe template/mariadb-ephemeral -n openshift</b>
Name:		mariadb-ephemeral
Namespace:	openshift
Created:	2 days ago
Labels:		samples.operator.openshift.io/managed=true
Description:	MariaDB database service, without persistent storage. For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/mariadb-container/blob/master/10.3/root/usr/share/container-scripts/mysql/README.md.
		
		WARNING: Any data stored will be lost upon pod destruction. Only use this template for testing
Annotations:	iconClass=icon-mariadb
		openshift.io/display-name=MariaDB (Ephemeral)
		openshift.io/documentation-url=https://github.com/scloc describe template/mariadb-ephemeral -n openshiftorg/mariadb-container/blob/master/10.3/root/usr/share/container-scripts/mysql/README.md
		openshift.io/long-description=This template provides a standalone MariaDB server with a database created.  The database is not stored on persistent storage, so any restart of the service will result in all data being lost.  The database name, username, and password are chosen via parameters when provisioning this service.
		openshift.io/provider-display-name=Red Hat, Inc.
		openshift.io/support-url=https://access.redhat.com
		samples.operator.openshift.io/version=4.10.5
		tags=database,mariadb

Parameters:		 
    Name:		MEMORY_LIMIT
    Display Name:	Memory Limit
    Description:	Maximum amount of memory the container can use.
    Required:		true
    Value:		512Mi

    Name:		NAMESPACE
    Display Name:	Namespace
    Description:	The OpenShift Namespace where the ImageStream resides.
    Required:		false
    Value:		openshift

    Name:		DATABASE_SERVICE_NAME
    Display Name:	Database Service Name
    Description:	The name of the OpenShift Service exposed for the database.
    Required:		true
    Value:		mariadb

    Name:		MYSQL_USER
    Display Name:	MariaDB Connection Username
    Description:	Username for MariaDB user that will be used for accessing the database.
    Required:		true
    Generated:		expression
    From:		user[A-Z0-9]{3}

    Name:		MYSQL_PASSWORD
    Display Name:	MariaDB Connection Password
    Description:	Password for the MariaDB connection user.
    Required:		true
    Generated:		expression
    From:		[a-zA-Z0-9]{16}

    Name:		MYSQL_ROOT_PASSWORD
    Display Name:	MariaDB root Password
    Description:	Password for the MariaDB root user.
    Required:		true
    Generated:		expression
    From:		[a-zA-Z0-9]{16}

    Name:		MYSQL_DATABASE
    Display Name:	MariaDB Database Name
    Description:	Name of the MariaDB database accessed.
    Required:		true
    Value:		sampledb

    Name:		MARIADB_VERSION
    Display Name:	Version of MariaDB Image
    Description:	Version of MariaDB image to be used (10.3-el7, 10.3-el8, or latest).
    Required:		true
    Value:		10.3-el8


Object Labels:	template=mariadb-ephemeral-template

Message:	The following service(s) have been created in your project: ${DATABASE_SERVICE_NAME}.
		
		       Username: ${MYSQL_USER}
		       Password: ${MYSQL_PASSWORD}
		  Database Name: ${MYSQL_DATABASE}
		 Connection URL: mysql://${DATABASE_SERVICE_NAME}:3306/
		
		For more information about using this template, including OpenShift considerations, see https://github.com/sclorg/mariadb-container/blob/master/10.3/root/usr/share/container-scripts/mysql/README.md.

Objects:				 
    Secret				${DATABASE_SERVICE_NAME}
    Service				${DATABASE_SERVICE_NAME}
    DeploymentConfig.apps.openshift.io	${DATABASE_SERVICE_NAME}
</pre>

Execute the below commands in the order mentioned to deploy the multi-pod wordpress application
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
<pre>
<b>username</b> - root or the userxxx shown in the terminal
<b>password</b> - root or connection password shown in the terminal
<b>hostname</b> - mariadb.ocp.svc.cluster.local or mariadb
<b>database</b> - sampledb
</pre>

The hostname you need to replace it with the hostname you see in Administrator --> Networking --> Services --> mariadb --> Hostname.
The DB hostname is nothing but the service name of mariadb DeploymentConfig.

## What is a Custom Resource in Kubernetes/OpenShift ?to manage CR i.e Custom Resource that we added into the OpenShift Cluster, we need to create Custom Controllers
- an API extension mechanism in Kubernetes/OpenShift
- helps you add a new kind of object in your Kubernetes/OpenShift Cluster 
  just like Deployment, ReplicaSet, Pod, etc.,
- a Custom Resource Definition(CRD) defines a Custom Resource(CR)
- once a CR is created using CRD it can be accessed using kubectl or oc commands
- Existing Kubernetes/OpenShift Controllers will not be able to manage the Custom Resource we added
- Hence, we also need to create Custom Controllers that can manage the Custom Resource for self-healing, scale up/down, etc.,

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

Below is a short list of Operator Frameworks available, but there are many more that I haven't listed here

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

You can also see the logs of the recently executed taskrun as shown below
```
tkn taskrun logs --last -f
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs --last -f</b>
[step1] Message from Step1

[step2] Message from Step2
</pre>

## ⛹️‍♀️ Lab - Executing bash commands in a task shell

Create a task manifest file named hello-bash.yml with the below content
<pre>
apiVersion: tekton.dev/v1beta1
kind: Task
metadata:
  name: hello-bash
  namespace: tektutor
spec:
  steps:
  - image: ubuntu # contains bash
    script: |
      #!/usr/bin/env bash
      echo "Hello from Bash!"
</pre>

Let's create the task
```
oc apply -f hello-bash.yml
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f hello-bash-task.yml</b>
task.tekton.dev/hello-bash created
</pre>

Let's list the task
```
tkn task list
```
Expected output
<pre>
(jegan@tektutor.org)$ tkn task list
NAME                             DESCRIPTION   AGE
hello                                          6 hours ago
<b>hello-bash                                     3 seconds ago</b>
hello-task-with-multiple-steps                 59 minutes ago
hello-task-with-params                         5 hours ago
</pre>

Let's run the task
```
tkn task start hello-bash
```
Expected output 
<pre>
(jegan@tektutor.org)$ <b>tkn task start hello-bash</b>
TaskRun started: hello-bash-run-9b86p

In order to track the TaskRun progress run:
tkn taskrun logs hello-bash-run-9b86p -f -n tektutor
</pre>

Let's check the output
```
tkn taskrun logs hello-bash-run-9b86p -f -n tektutor
```
Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs hello-bash-run-9b86p -f -n tektutor</b>
[unnamed-0] Hello from Bash!
</pre>

## How to control the task execution if a step errored out and you know it isn't a show-stopper?

Create the task
```
cd ~
cd openshift-tekton-march-2022
git pull
cd Day4
oc apply -f task-with-onerror.yml
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task-with-onerror.yml</b>
task.tekton.dev/task-with-onerror created
</pre>

List the task
```
tkn task list
```
Expected output is
<pre>
(jegan@tektutor.org)$ tkn task list
NAME                             DESCRIPTION   AGE
hello                                          7 hours ago
hello-bash                                     56 minutes ago
hello-task-with-multiple-steps                 1 hour ago
hello-task-with-params                         6 hours ago
<b>task-with-onerror                              53 seconds ago</b>
</pre>

Execute the task
```
tkn task task-with-onerror
```
Expected output is
<pre>
(jegan@tektutor.org)$ tkn task start task-with-onerror
TaskRun started: task-with-onerror-run-mkxg7

In order to track the TaskRun progress run:
tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor
</pre>

Check the output
```
tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor
```


Hit Enter key to see the output 
<pre>
(jegan@tektutor.org)$ <b>tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor</b>


[step1-with-error] go: go.mod file not found in current directory or any parent directory; see 'go help modules'

[step2-after-erro] echo \"Inspite of step had reported an error, this step2 executed !\"
</pre>

## Another task that ignores and produces result
```
cd ~
cd openshift-tekton-march-2022
git pull
cd Day4
oc apply -f task-with-onerror.yml 
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f task-with-onerror.yml</b>
task.tekton.dev/task-with-onerror created
</pre>

List the task
```
tkn task list
```
Expected output is
<pre>
jegan@tektutor.org)$ tkn task list
NAME                             DESCRIPTION   AGE
hello                                          7 hours ago
hello-bash                                     56 minutes ago
hello-python                                   1 hour ago
hello-task-with-multiple-steps                 1 hour ago
hello-task-with-params                         6 hours ago
<b>task-with-onerror                              53 seconds ago</b>
</pre>

Execute the task
```
tkn task start task-with-onerror
```
Expected outputs
<pre>
jegan@tektutor.org)$ tkn task start task-with-onerror
TaskRun started: task-with-onerror-run-mkxg7

In order to track the TaskRun progress run:
tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor
</pre>

Check the output
```
tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor
```
Expected output
<pre>
(jegan@tektutor.org)$ tkn taskrun logs task-with-onerror-run-mkxg7 -f -n tektutor


[step1-with-error] go: go.mod file not found in current directory or any parent directory; see 'go help modules'

[step2-after-erro] 2022/03/29 14:27:02 Error executing command: exec: "echo \"Inspite of step had reported an error, this step2 executed !\"": executable file not found in $PATH
</pre>


## Task that clones github repository
We need to install the git-clone task from Tekton Hub.  For more details refer https://hub.tekton.dev/tekton/task/git-clone

```
tkn hub install task git-clone
```

Expected output is

<pre>
(jegan@tektutor.org)$ <b>tkn hub install task git-clone</b>
Task git-clone(0.5) installed in tektutor namespace
</pre>

