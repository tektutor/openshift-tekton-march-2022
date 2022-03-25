# CI/CD in RedHat OpenShift with Tekton

For training/consulting/coaching, you may reach me
<pre>
    jegan@tektutor.org
    +91 822-000-5626 (WhatsApp)
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

</pre>
