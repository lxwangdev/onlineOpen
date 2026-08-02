# Pod OS, Controllers, and Scheduling (Part 03 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

### Pod OS

> **FEATURE STATE: `Kubernetes v1.25 [stable]`

You should set the `.spec.os.name` field to either `windows` or `linux` to indicate the operating system that the containers in that Pod require. These two are the only operating systems supported for now by Kubernetes. In the future, this list may be expanded.

The kubelet refuses to run a Pod if the value of `.spec.os.name` does not match the operating system of the node. However, in Kubernetes v1.36, the value of `.spec.os.name` does not affect how the [kube-scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/) picks a node for the Pod to run on. In any cluster where there is more than one operating system for running nodes, you should set the [kubernetes.io/os](https://kubernetes.io/docs/reference/labels-annotations-taints/#kubernetes-io-os) label correctly on each node, and define pods with a `nodeSelector` based on the operating system label. The kube-scheduler assigns your pod to a node based on other criteria and may or may not succeed in picking a suitable node placement where the node OS is right for the containers in that Pod. The [Pod security standards](https://kubernetes.io/docs/concepts/security/pod-security-standards/) also use this field to avoid enforcing policies that aren't relevant to the operating system.

### Pods and controllers

You can use workload resources to create and manage multiple Pods for you. A controller for the resource handles replication and rollout and automatic healing in case of Pod failure. For example, if a Node fails, a controller notices that Pods on that Node have stopped working and creates a replacement Pod. The scheduler places the replacement Pod onto a healthy Node.

Here are some examples of workload resources that manage one or more Pods:

- [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
- [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset)

### Specifying a scheduling group

> **FEATURE STATE: `Kubernetes v1.35 [alpha]` (disabled by default) *(Feature Gate: GenericWorkload)*

By default, Kubernetes schedules every Pod individually. However, some tightly-coupled applications need a group of Pods to be scheduled simultaneously to function correctly.

You can link a Pod to a [PodGroup](https://kubernetes.io/docs/concepts/workloads/podgroup-api/) using the [scheduling group](https://kubernetes.io/docs/concepts/workloads/pods/scheduling-group/) field (`spec.schedulingGroup`). This tells the `kube-scheduler` that the `Pod` belongs to a specific group, enabling it to apply group-level coordinated placement decisions for the entire group at once.
