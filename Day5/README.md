## ⛹️ Lab - Task that clones github repository
We need create a PersistentVolume and PersistentVolumeClaim. Though we could technically use EmptyDir volume.  Using EmptyDir, will only store the data temporarily while PersistentVolume is considered permanent.

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
