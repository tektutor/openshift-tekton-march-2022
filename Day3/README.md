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
