# OpenShift

## ⛹️‍♂️ Lab - Listing images in OpenShift
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
Let's create a new project before deploying our application

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

Now let's deploy our application from TekTutor GitHub Repository
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

Let's us check the status of the application deployment
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


## ⛹️‍♂️ Lab - Deploying mysql db server using Docker Image with parameters
```
oc new-apply docker.io/mysql:latest -e MYSQL_ROOT_PASSWORD=root
```

The expected output is
<pre>
(jegan@tektutor.org)$ oc new-app docker.io/mysql:latest -e MYSQL_ROOT_PASSWORD=root
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
When prompted for mysql root password, type 'root' without quotes a the password.

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
