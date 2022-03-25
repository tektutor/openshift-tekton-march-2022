# CI/CD in RedHat OpenShift with Tekton

For training/consulting/coaching, you may reach me
<pre>
    jegan@tektutor.org
    +91 822-000-5626 (WhatsApp)
</pre>

# What is an Operator in Kubernetes/OpenShift?
- is a method of packaging, deploying and managing a Kubernetes/OpenShift application
- takes human operational knowledge and encodes it into software that is more easily packaged and shared with consumers. 
- is an extension of the software vendor’s engineering team that watches over your Kubernetes/Openshift environment and uses its current state to make decisions in milliseconds
- is designed to handle upgrades seamlessly, react to failures automatically, and not take shortcuts, like skipping a software backup process to save time

# Install Operator SDK

We need to install these software for Operator SDK to work
1. Go
2. Operator SDK
3. Ansible
4. Ansible Runner
5. Ansible Runner Http Event Emitter
6. OpenShift Python client

## Installing Go Programming Language
For other Operating Systems, check the official website here https://go.dev/dl/

To install Go in Linux
```
cd ~/Downloads
wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
tar xvfz go1.18.linux-amd64.tar.gz
```

Make sure the path of go extracted path is added to your .bashrc file
```
export PATH=/home/jegan/Downloads/go/bin:$PATH
```
To apply changes made in .bashrc file
```
source ~/.bashrc
```

Check if go works
```
go version
```

Expected output is
<pre>
(jegan@tektutor.org)$ <b>go version</b>
go version go1.18 linux/amd64
</pre>

## Installing Operator SDK
The official website is https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/operator-sdk/4.9.0/

```
cd ~/Downloads
wget https://mirror.openshift.com/pub/openshift-v4/x86_64/clients/operator-sdk/4.9.0/operator-sdk-v1.10.1-ocp-linux-x86_64.tar.gz
tar xvfz operator-sdk-v1.10.1-ocp-linux-x86_64.tar.gz
chmod +x ./operator-sdk
sudo mv operator-sdk /usr/local/bin
```

Now check the version of operator-sdk
```
operator-sdk version
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>operator-sdk version</b>
operator-sdk version: "v1.10.1-ocp", commit: "58bf7213d22688d7d0cd4ccc7f4faf4ec5b33e3f", kubernetes version: "v1.21", go version: "go1.16.6", GOOS: "linux", GOARCH: "amd64"
</pre>

## Installing Ansible
```
sudo yum install ansible
```

Check if ansible is working
```
ansible --verison
```

Expected output is

<pre>
(jegan@tektutor.org)$ <b>ansible --version</b>
ansible 2.9.27
  config file = /etc/ansible/ansible.cfg
  configured module search path = [u'/home/jegan/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python2.7/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 2.7.17 (default, Feb 27 2021, 15:10:58) [GCC 7.5.0]
</pre>

## Installing Ansible Runner
More details can be found here https://ansible-runner.readthedocs.io/en/latest/install/

```
pip install ansible-runner
```

## Installing Ansible Runner Http Event Emitter
```
cd ~
git clone https://github.com/ansible/ansible-runner-http.git
cd 
```

## Installing Openshift Python client
```
pip install openshift
```
Expected output is
<pre>
(jegan@tektutor.org)$ <b>pip install openshift</b>
Collecting openshift
  Downloading https://files.pythonhosted.org/packages/97/c0/d8e2aae7b4e8f3709eca4fd8c2f70ea3c66151d1a5259e9a7e1ee2497608/openshift-0.13.1.tar.gz
Collecting kubernetes>=12.0 (from openshift)
  Downloading https://files.pythonhosted.org/packages/19/4a/39b09950b35a36fe1af54bb85413c2976b62a5c29e7254c8c3573f86c028/kubernetes-18.20.0-py2.py3-none-any.whl (1.6MB)
    100% |████████████████████████████████| 1.6MB 679kB/s 
Collecting python-string-utils (from openshift)
  Downloading https://files.pythonhosted.org/packages/5d/13/216f2d4a71307f5a4e5782f1f59e6e8e5d6d6c00eaadf9f92aeccfbb900c/python-string-utils-0.6.0.tar.gz
Collecting six (from openshift)
  Using cached https://files.pythonhosted.org/packages/d9/5a/e7c31adbe875f2abbb91bd84cf2dc52d792b5a01506781dbcf25c91daf11/six-1.16.0-py2.py3-none-any.whl
Collecting setuptools>=21.0.0 (from kubernetes>=12.0->openshift)
  Using cached https://files.pythonhosted.org/packages/e1/b7/182161210a13158cd3ccc41ee19aadef54496b74f2817cc147006ec932b4/setuptools-44.1.1-py2.py3-none-any.whl
Collecting ipaddress>=1.0.17; python_version == "2.7" (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/c2/f8/49697181b1651d8347d24c095ce46c7346c37335ddc7d255833e7cde674d/ipaddress-1.0.23-py2.py3-none-any.whl
Collecting requests (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/2d/61/08076519c80041bc0ffa1a8af0cbd3bf3e2b62af10435d269a9d0f40564d/requests-2.27.1-py2.py3-none-any.whl (63kB)
    100% |████████████████████████████████| 71kB 4.2MB/s 
Collecting python-dateutil>=2.5.3 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/36/7a/87837f39d0296e723bb9b62bbb257d0355c7f6128853c78955f57342a56d/python_dateutil-2.8.2-py2.py3-none-any.whl (247kB)
    100% |████████████████████████████████| 256kB 2.4MB/s 
Collecting requests-oauthlib (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/6f/bb/5deac77a9af870143c684ab46a7934038a53eb4aa975bc0687ed6ca2c610/requests_oauthlib-1.3.1-py2.py3-none-any.whl
Collecting websocket-client!=0.40.0,!=0.41.*,!=0.42.*,>=0.32.0 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/f7/0c/d52a2a63512a613817846d430d16a8fbe5ea56dd889e89c68facf6b91cb6/websocket_client-0.59.0-py2.py3-none-any.whl (67kB)
    100% |████████████████████████████████| 71kB 4.6MB/s 
Collecting certifi>=14.05.14 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/37/45/946c02767aabb873146011e665728b680884cd8fe70dde973c640e45b775/certifi-2021.10.8-py2.py3-none-any.whl (149kB)
    100% |████████████████████████████████| 153kB 3.2MB/s 
Collecting urllib3>=1.24.2 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/ec/03/062e6444ce4baf1eac17a6a0ebfe36bb1ad05e1df0e20b110de59c278498/urllib3-1.26.9-py2.py3-none-any.whl (138kB)
    100% |████████████████████████████████| 143kB 3.4MB/s 
Collecting google-auth>=1.0.1 (from kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/21/6c/e784c6883689fae9d1c5f82d995ddc82c284daa514edd5ba67ea38d88341/google_auth-2.6.2-py2.py3-none-any.whl (156kB)
    100% |████████████████████████████████| 163kB 3.5MB/s 
Collecting pyyaml>=5.4.1 (from kubernetes>=12.0->openshift)
  Using cached https://files.pythonhosted.org/packages/ba/d4/3cf562876e0cda0405e65d351b835077ab13990e5b92912ef2bf1a2280e0/PyYAML-5.4.1-cp27-cp27mu-manylinux1_x86_64.whl
Collecting idna<3,>=2.5; python_version < "3" (from requests->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/a2/38/928ddce2273eaa564f6f50de919327bf3a00f091b5baba8dfa9460f3a8a8/idna-2.10-py2.py3-none-any.whl (58kB)
    100% |████████████████████████████████| 61kB 5.8MB/s 
Collecting chardet<5,>=3.0.2; python_version < "3" (from requests->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/19/c7/fa589626997dd07bd87d9269342ccb74b1720384a4d739a1872bd84fbe68/chardet-4.0.0-py2.py3-none-any.whl (178kB)
    100% |████████████████████████████████| 184kB 2.8MB/s 
Collecting oauthlib>=3.0.0 (from requests-oauthlib->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/05/57/ce2e7a8fa7c0afb54a0581b14a65b56e62b5759dbc98e80627142b8a3704/oauthlib-3.1.0-py2.py3-none-any.whl (147kB)
    100% |████████████████████████████████| 153kB 2.9MB/s 
Collecting pyasn1-modules>=0.2.1 (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/95/de/214830a981892a3e286c3794f41ae67a4495df1108c3da8a9f62159b9a9d/pyasn1_modules-0.2.8-py2.py3-none-any.whl (155kB)
    100% |████████████████████████████████| 163kB 3.0MB/s 
Collecting cachetools<6.0,>=2.0.0 (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/2f/a6/30b0a0bef12283e83e58c1d6e7b5aabc7acfc4110df81a4471655d33e704/cachetools-3.1.1-py2.py3-none-any.whl
Collecting rsa<4.6; python_version < "3.6" (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/26/f8/8127fdda0294f044121d20aac7785feb810e159098447967a6103dedfb96/rsa-4.5-py2.py3-none-any.whl
Collecting enum34>=1.1.10; python_version < "3.4" (from google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/6f/2c/a9386903ece2ea85e9807e0e062174dc26fdce8b05f216d00491be29fad5/enum34-1.1.10-py2-none-any.whl
Collecting pyasn1<0.5.0,>=0.4.6 (from pyasn1-modules>=0.2.1->google-auth>=1.0.1->kubernetes>=12.0->openshift)
  Downloading https://files.pythonhosted.org/packages/62/1e/a94a8d635fa3ce4cfc7f506003548d0a2447ae76fd5ca53932970fe3053f/pyasn1-0.4.8-py2.py3-none-any.whl (77kB)
    100% |████████████████████████████████| 81kB 3.8MB/s 
Building wheels for collected packages: openshift, python-string-utils
  Running setup.py bdist_wheel for openshift ... done
  Stored in directory: /home/jegan/.cache/pip/wheels/31/5c/3f/c655e9f2030f180a983293feeec85ff95d2546ae7d3da6cb41
  Running setup.py bdist_wheel for python-string-utils ... done
  Stored in directory: /home/jegan/.cache/pip/wheels/b2/1d/3f/2f554103ba3e4aa8d53b8e1ea70b5a0b4f74409a789e17fa35
Successfully built openshift python-string-utils
Installing collected packages: setuptools, ipaddress, idna, certifi, urllib3, chardet, requests, six, python-dateutil, oauthlib, requests-oauthlib, websocket-client, pyasn1, pyasn1-modules, cachetools, rsa, enum34, google-auth, pyyaml, kubernetes, python-string-utils, openshift
Successfully installed cachetools-3.1.1 certifi-2021.10.8 chardet-4.0.0 enum34-1.1.10 google-auth-2.6.2 idna-2.10 ipaddress-1.0.23 kubernetes-18.20.0 oauthlib-3.1.0 openshift-0.13.1 pyasn1-0.4.8 pyasn1-modules-0.2.8 python-dateutil-2.8.2 python-string-utils-0.6.0 pyyaml-5.4.1 requests-2.27.1 requests-oauthlib-1.3.1 rsa-4.5 setuptools-44.1.1 six-1.16.0 urllib3-1.26.9 websocket-client-0.59.0
</pre>


# Extending OpenShift API by adding our own Custom Resource

Let's create the CustomResourceDefinition to add a new type of Resource called Training

```
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: trainings.tektutor.org 
spec:
  group: tektutor.org 
  versions:
    - name: v1
      served: true
      storage: true
      schema:
        openAPIV3Schema:
          type: object
          properties:
            training:
               type: string
            duration:
               type: string
            city:
               type: string
            from:
               type: string
            to:
               type: string
          required:
            - training
            - duration
            - city
            - from
            - to
  conversion:
    strategy: None
  scope: Namespaced
  names:
    kind: Training 
    listKind: TrainingList
    plural: trainings 
    singular: training 
    shortNames:
    - train 
```

Let's create the training crd as shown below
```
git clone https://github.com/tektutor/openshift-tekton-march-2022.git
cd Day3
oc apply -f training-crd.yml
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f training-crd.yml</b>
customresourcedefinition.apiextensions.k8s.io/trainings.tektutor.org created
</pre>

Let's list the crds present in the OpenShift cluster and see if our CRD is there
```
oc get crds
```

The expected output is

<pre>
(jegan@tektutor.org)$ oc get crd
NAME                                                              CREATED AT
addresspools.metallb.io                                           2022-03-24T14:11:35Z
alertmanagerconfigs.monitoring.coreos.com                         2022-03-24T13:07:12Z
alertmanagers.monitoring.coreos.com                               2022-03-24T13:07:15Z
apirequestcounts.apiserver.openshift.io                           2022-03-24T13:15:15Z
apiservers.config.openshift.io                                    2022-03-24T13:06:41Z
authentications.config.openshift.io                               2022-03-24T13:06:41Z
authentications.operator.openshift.io                             2022-03-24T13:07:15Z
baremetalhosts.metal3.io                                          2022-03-24T13:07:10Z
bfdprofiles.metallb.io                                            2022-03-24T14:11:35Z
bgppeers.metallb.io                                               2022-03-24T14:11:36Z
bmceventsubscriptions.metal3.io                                   2022-03-24T13:07:12Z
builds.config.openshift.io                                        2022-03-24T13:06:42Z
catalogsources.operators.coreos.com                               2022-03-24T13:07:13Z
cloudcredentials.operator.openshift.io                            2022-03-24T13:07:00Z
clusterautoscalers.autoscaling.openshift.io                       2022-03-24T13:07:12Z
clustercsidrivers.operator.openshift.io                           2022-03-24T13:07:49Z
clusternetworks.network.openshift.io                              2022-03-24T13:12:43Z
clusteroperators.config.openshift.io                              2022-03-24T13:06:29Z
clusterresourcequotas.quota.openshift.io                          2022-03-24T13:06:40Z
clusterserviceversions.operators.coreos.com                       2022-03-24T13:07:15Z
clusterversions.config.openshift.io                               2022-03-24T13:06:29Z
configs.imageregistry.operator.openshift.io                       2022-03-24T13:07:12Z
configs.operator.openshift.io                                     2022-03-24T13:07:19Z
configs.samples.operator.openshift.io                             2022-03-24T13:07:11Z
consoleclidownloads.console.openshift.io                          2022-03-24T13:07:10Z
consoleexternalloglinks.console.openshift.io                      2022-03-24T13:07:10Z
consolelinks.console.openshift.io                                 2022-03-24T13:07:10Z
consolenotifications.console.openshift.io                         2022-03-24T13:07:10Z
consoleplugins.console.openshift.io                               2022-03-24T13:07:10Z
consolequickstarts.console.openshift.io                           2022-03-24T13:07:10Z
consoles.config.openshift.io                                      2022-03-24T13:06:42Z
consoles.operator.openshift.io                                    2022-03-24T13:07:10Z
consoleyamlsamples.console.openshift.io                           2022-03-24T13:07:10Z
containerruntimeconfigs.machineconfiguration.openshift.io         2022-03-24T13:07:30Z
controllerconfigs.machineconfiguration.openshift.io               2022-03-24T13:14:13Z
credentialsrequests.cloudcredential.openshift.io                  2022-03-24T13:06:59Z
csisnapshotcontrollers.operator.openshift.io                      2022-03-24T13:07:12Z
dnses.config.openshift.io                                         2022-03-24T13:06:42Z
dnses.operator.openshift.io                                       2022-03-24T13:07:18Z
dnsrecords.ingress.operator.openshift.io                          2022-03-24T13:07:14Z
egressnetworkpolicies.network.openshift.io                        2022-03-24T13:12:43Z
egressrouters.network.operator.openshift.io                       2022-03-24T13:07:23Z
etcds.operator.openshift.io                                       2022-03-24T13:07:09Z
featuregates.config.openshift.io                                  2022-03-24T13:06:43Z
firmwareschemas.metal3.io                                         2022-03-24T13:07:15Z
helmchartrepositories.helm.openshift.io                           2022-03-24T13:07:09Z
hostfirmwaresettings.metal3.io                                    2022-03-24T13:07:18Z
hostsubnets.network.openshift.io                                  2022-03-24T13:12:43Z
imagecontentpolicies.config.openshift.io                          2022-03-24T13:06:44Z
imagecontentsourcepolicies.operator.openshift.io                  2022-03-24T13:06:44Z
imagepruners.imageregistry.operator.openshift.io                  2022-03-24T13:07:43Z
images.config.openshift.io                                        2022-03-24T13:06:43Z
infrastructures.config.openshift.io                               2022-03-24T13:06:44Z
ingresscontrollers.operator.openshift.io                          2022-03-24T13:07:03Z
ingresses.config.openshift.io                                     2022-03-24T13:06:45Z
installplans.operators.coreos.com                                 2022-03-24T13:07:19Z
ippools.whereabouts.cni.cncf.io                                   2022-03-24T13:12:42Z
kubeapiservers.operator.openshift.io                              2022-03-24T13:07:38Z
kubecontrollermanagers.operator.openshift.io                      2022-03-24T13:07:14Z
kubeletconfigs.machineconfiguration.openshift.io                  2022-03-24T13:07:32Z
kubeschedulers.operator.openshift.io                              2022-03-24T13:07:14Z
kubestorageversionmigrators.operator.openshift.io                 2022-03-24T13:07:10Z
machineautoscalers.autoscaling.openshift.io                       2022-03-24T13:07:15Z
machineconfigpools.machineconfiguration.openshift.io              2022-03-24T13:07:36Z
machineconfigs.machineconfiguration.openshift.io                  2022-03-24T13:07:33Z
machinehealthchecks.machine.openshift.io                          2022-03-24T13:07:51Z
machines.machine.openshift.io                                     2022-03-24T13:07:49Z
machinesets.machine.openshift.io                                  2022-03-24T13:07:49Z
metallbs.metallb.io                                               2022-03-24T14:11:35Z
netnamespaces.network.openshift.io                                2022-03-24T13:12:43Z
network-attachment-definitions.k8s.cni.cncf.io                    2022-03-24T13:12:42Z
networks.config.openshift.io                                      2022-03-24T13:06:45Z
networks.operator.openshift.io                                    2022-03-24T13:07:15Z
oauths.config.openshift.io                                        2022-03-24T13:06:46Z
olmconfigs.operators.coreos.com                                   2022-03-24T13:07:26Z
openshiftapiservers.operator.openshift.io                         2022-03-24T13:07:09Z
openshiftcontrollermanagers.operator.openshift.io                 2022-03-24T13:07:15Z
operatorconditions.operators.coreos.com                           2022-03-24T13:07:27Z
operatorgroups.operators.coreos.com                               2022-03-24T13:07:30Z
operatorhubs.config.openshift.io                                  2022-03-24T13:06:40Z
operatorpkis.network.operator.openshift.io                        2022-03-24T13:07:24Z
operators.operators.coreos.com                                    2022-03-24T13:07:33Z
overlappingrangeipreservations.whereabouts.cni.cncf.io            2022-03-24T13:12:42Z
podmonitors.monitoring.coreos.com                                 2022-03-24T13:07:18Z
podnetworkconnectivitychecks.controlplane.operator.openshift.io   2022-03-24T13:45:31Z
preprovisioningimages.metal3.io                                   2022-03-24T13:07:20Z
probes.monitoring.coreos.com                                      2022-03-24T13:07:20Z
profiles.tuned.openshift.io                                       2022-03-24T13:07:15Z
projects.config.openshift.io                                      2022-03-24T13:06:46Z
prometheuses.monitoring.coreos.com                                2022-03-24T13:07:23Z
prometheusrules.monitoring.coreos.com                             2022-03-24T13:07:25Z
provisionings.metal3.io                                           2022-03-24T13:07:49Z
proxies.config.openshift.io                                       2022-03-24T13:06:40Z
rangeallocations.security.internal.openshift.io                   2022-03-24T13:06:40Z
rolebindingrestrictions.authorization.openshift.io                2022-03-24T13:06:40Z
schedulers.config.openshift.io                                    2022-03-24T13:06:46Z
securitycontextconstraints.security.openshift.io                  2022-03-24T13:06:40Z
servicecas.operator.openshift.io                                  2022-03-24T13:07:18Z
servicemonitors.monitoring.coreos.com                             2022-03-24T13:07:27Z
storages.operator.openshift.io                                    2022-03-24T13:07:50Z
storagestates.migration.k8s.io                                    2022-03-24T13:07:14Z
storageversionmigrations.migration.k8s.io                         2022-03-24T13:07:11Z
subscriptions.operators.coreos.com                                2022-03-24T13:07:47Z
thanosrulers.monitoring.coreos.com                                2022-03-24T13:07:30Z
<b>trainings.tektutor.org                                            2022-03-25T00:32:07Z</b>
tuneds.tuned.openshift.io                                         2022-03-24T13:07:18Z
volumesnapshotclasses.snapshot.storage.k8s.io                     2022-03-24T13:15:14Z
volumesnapshotcontents.snapshot.storage.k8s.io                    2022-03-24T13:15:13Z
volumesnapshots.snapshot.storage.k8s.io                           2022-03-24T13:15:12Z
</pre>

Now let's create a new Training in OpenShift
```
cd openshift-tekton-march-2022
git pull
cd Day3
oc apply -f training.yml
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc apply -f training.yml</b>
training.tektutor.org/openshift-training created
</pre>

Let's now try listing the trainings

```
oc get trainings
oc get training
oc get train
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc get training</b>
NAME                 AGE
openshift-training   5s
</pre>

Let us find more details about the training custom resource
```
oc describe training/openshift-training
```

The expected output is
<pre>
(jegan@tektutor.org)$ <b>oc describe training/openshift-training</b>
Name:         openshift-training
Namespace:    tektutor
Labels:       <none>
Annotations:  <none>
API Version:  tektutor.org/v1
Kind:         Training
Metadata:
  Creation Timestamp:  2022-03-25T01:31:45Z
  Generation:          1
  Managed Fields:
    API Version:  tektutor.org/v1
    Fields Type:  FieldsV1
    fieldsV1:
      f:metadata:
        f:annotations:
          .:
          f:kubectl.kubernetes.io/last-applied-configuration:
    Manager:         kubectl-client-side-apply
    Operation:       Update
    Time:            2022-03-25T01:31:45Z
  Resource Version:  153931
  UID:               25fed033-87d2-4aff-b789-feb2ef980bb6
Events:              <none>
</pre>
