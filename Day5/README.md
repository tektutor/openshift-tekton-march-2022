## ⛹️ Lab - Task that clones github repository
We need to create a PersistentVolume and PersistentVolumeClaim. Though we could technically use EmptyDir volume.  Using EmptyDir, will only store the data temporarily while PersistentVolume is considered permanent.

This lab assumes, you already have installed NFS Server.  In my case the NFS Server is running in a machine with IP 192.168.1.80 on my local network.

Configured NFS Server /etc/exports file to export /mnt/maven folder.
```
/mnt/nfs_share  192.168.122.0/24(rw,sync,no_subtree_check)
/mnt/mysql      192.168.122.0/24(rw,sync,no_subtree_check)
/mnt/wordpress  192.168.122.0/24(rw,sync,no_subtree_check)
/mnt/maven	192.168.122.0/24(rw,sync,no_subtree_check)
```

Refresh the exports folders in NFS Server
```
sudo exportfs -ra
```

Double check if the newly added folder in the /etc/exports is really shared by NFS Server
```
sudo showmount -e 192.168.1.80
```

Expected output is
<pre>
(jegan@tektutor.org)$ sudo showmount -e 192.168.1.80
Export list for 192.168.1.80:
/mnt/maven     192.168.122.0/24
/mnt/wordpress 192.168.122.0/24
/mnt/mysql     192.168.122.0/24
/mnt/nfs_share 192.168.122.0/24
</pre>


Tekton Persistent Volume - openshift-tekton-pv.yml
```
# Reserve 500 MB disk space for Persistent Volume
apiVersion: v1
kind: PersistentVolume
metadata:
  name: openshift-tekton-pv
  labels:
     app: tekton
spec:
  capacity:
    storage: 500Mi
  volumeMode: Filesystem
  accessModes:
  - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain 
  nfs: 
    server: "192.168.1.80"
    path: "/mnt/maven"
```

Create the Persistent Volume
```
oc apply -f openshift-tekton-pv.yml
```

TekTon Persistent Volume Claim - openshift-tekton-pvc.yml
```
# Request for 500 disk space from one of the Persistent Volume
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: openshift-tekton-pvc
  labels:
     app: tekton
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 500Mi
```

Create the Persistent Volume Claim
```
oc apply -f openshift-tekton-pvc.yml
```

We need to install the git-clone task from Tekton Hub.  For more details refer https://hub.tekton.dev/tekton/task/git-clone
```
tkn hub install task git-clone
```

Expected output is

<pre>
(jegan@tektutor.org)$ <b>tkn hub install task git-clone</b>
Task git-clone(0.5) installed in tektutor namespace
</pre>

Now let's create the TaskRun that clones the GitHub Repo.  Let's create a file named tekton-gitclone-taskrun.yml
```
apiVersion: tekton.dev/v1beta1
kind: TaskRun
metadata:
  generateName: github-clone-taskrun-
spec:
  taskRef:
    kind: Task
    name: git-clone
  params:
    - name: url 
      value: 'https://github.com/tektutor/spring-ms.git'
    - name: revision
      value: master
  workspaces:
    - name: output
      persistentVolumeClaim:
        claimName: openshift-tekton-pvc
```

You may now create the taskrun as shown below
```
oc create -f tekton-gitclone-taskrun.yml
```

You can check the status in CLI
```
tkn taskrun list
tkn taskrun logs -f --last
```

You can also watch the status and output in OpenShift webconsole.
OpenShift webconsole --> Pipelines --> Task --> TaskRun(Tab) and select github-clone-taskrun-xxyyzz

## ⛹️‍♀️ Lab - Creating your first pipeline
```
cd ~
cd openshift-tekton-march-2022
git pull
cd Day5
oc create -f first-pipeline.yml
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc create -f first-pipeline.yml</b>
task.tekton.dev/task1 created
task.tekton.dev/task2 created
pipeline.tekton.dev/first-pipeline created
</pre>

You can then list and see if the pipeline is created successfully with one of the below commands.
```
tkn pipelines list
tkn pipeline list
tkn p list
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn p list</b>
NAME             AGE              LAST RUN   STARTED   DURATION   STATUS
first-pipeline   13 seconds ago   ---        ---       ---        ---
</pre>


You may start running the pipeline as shown below
```
tkn p start first-pipeline  --showlog
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>tkn p start first-pipeline --showlog</b>
PipelineRun started: first-pipeline-run-lkrrl
Waiting for logs to be available...
[task1 : step1] Step1

[task1 : step2] Step2

[task2 : step1] Step1

[task2 : step2] Step2

[task2 : step3] Step3
</pre>

## ⛹️‍♀️ Lab - Creating your second pipeline
```
cd ~
cd openshift-tekton-march-2022
git pull
cd Day5
oc create -f second-pipeline.yml
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f second-pipeline.yml</b>
task.tekton.dev/task3 created
task.tekton.dev/task4 created
task.tekton.dev/task5 created
pipeline.tekton.dev/second-pipeline created
</pre>

List and check if the second-pipeline is created successfully.
```
tkn p list
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn p list</b>
NAME              AGE              LAST RUN                STARTED          DURATION     STATUS
first-pipeline    52 minutes ago   first-pipeline-2f5v2u   45 minutes ago   36 seconds   Succeeded
second-pipeline   9 seconds ago    ---                     ---              ---          ---
</pre>

Start the pipeline execution
```
tkn pipeline start second-pipeline --showlog
```

Expected output
<pre>
(jegan@tektutor.org)$ <b>tkn pipeline start second-pipeline --showlog</b>
PipelineRun started: second-pipeline-run-sxrzv
Waiting for logs to be available...
[task1 : step1] Step1

[task1 : step2] Step2

[task2 : step1] Step1

[task2 : step2] Step2

[task2 : step3] Step3

[task3 : step1] Step1
[task4 : step1] Step1


[task5 : step1] Step1
</pre>
