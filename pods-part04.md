# Pod Templates (Part 04 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

### Pod templates

Controllers for [workload](https://kubernetes.io/docs/concepts/workloads/) resources create Pods from a *pod template* and manage those Pods on your behalf.

PodTemplates are specifications for creating Pods, and are included in workload resources such as [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), [Jobs](https://kubernetes.io/docs/concepts/workloads/controllers/job/), and [DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/).

Each controller for a workload resource uses the `PodTemplate` inside the workload object to make actual Pods. The `PodTemplate` is part of the desired state of whatever workload resource you used to run your app.

When you create a Pod, you can include [environment variables](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) in the Pod template for the containers that run in the Pod.

The sample below is a manifest for a simple Job with a `template` that starts one container. The container in that Pod prints a message then pauses.

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: hello
spec:
  template:
    # This is the pod template
    spec:
      containers:
      - name: hello
        image: busybox:1.28
        command: ['sh', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
      restartPolicy: OnFailure
    # The pod template ends here
```

Modifying the pod template or switching to a new pod template has no direct effect on the Pods that already exist. If you change the pod template for a workload resource, that resource needs to create replacement Pods that use the updated template.

For example, the StatefulSet controller ensures that the running Pods match the current pod template for each StatefulSet object. If you edit the StatefulSet to change its pod template, the StatefulSet starts to create new Pods based on the updated template. Eventually, all of the old Pods are replaced with new Pods, and the update is complete.

Each workload resource implements its own rules for handling changes to the Pod template. If you want to read more about StatefulSet specifically, read [Update strategy](https://kubernetes.io/docs/tutorials/stateful-application/basic-stateful-set/#updating-statefulsets) in the StatefulSet Basics tutorial.

On Nodes, the [kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet) does not directly observe or manage any of the details around pod templates and updates; those details are abstracted away. That abstraction and separation of concerns simplifies system semantics, and makes it feasible to extend the cluster's behavior without changing existing code.
