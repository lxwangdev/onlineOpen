# Container Probes and Next Steps (Part 10 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Container probes

A *probe* is a diagnostic performed periodically by the kubelet on a container. To perform a diagnostic, the kubelet can invoke different actions:

- `ExecAction` (performed with the help of the container runtime)
- `TCPSocketAction` (checked directly by the kubelet)
- `HTTPGetAction` (checked directly by the kubelet)

You can read more about [probes](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes) in the Pod Lifecycle documentation.

## What's next

- Learn about the [lifecycle of a Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/).
- Read about [PodDisruptionBudget](https://kubernetes.io/docs/concepts/workloads/pods/disruptions/) and how you can use it to manage application availability during disruptions.
- Pod is a top-level resource in the Kubernetes REST API. The [Pod](https://kubernetes.io/docs/reference/kubernetes-api/core/pod-v1/) object definition describes the object in detail.
- [The Distributed System Toolkit: Patterns for Composite Containers](https://kubernetes.io/blog/2015/06/the-distributed-system-toolkit-patterns/) explains common layouts for Pods with more than one container.
- Read about [Pod topology spread constraints](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/)
- Read [Advanced Pod Configuration](https://kubernetes.io/docs/concepts/workloads/pods/advanced-pod-config/) to learn the topic in detail. That page covers aspects of Pod configuration beyond the essentials, including:
  - PriorityClasses
  - RuntimeClasses
  - advanced ways to configure *scheduling*: the way that Kubernetes decides which node a Pod should run on.

To understand the context for why Kubernetes wraps a common Pod API in other resources (such as [StatefulSets](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) or [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)), you can read about the prior art, including:

- [Aurora](https://aurora.apache.org/documentation/latest/reference/configuration/#job-schema)
- [Borg](https://research.google/pubs/large-scale-cluster-management-at-google-with-borg/)
- [Marathon](https://github.com/d2iq-archive/marathon)
- [Omega](https://research.google/pubs/pub41684/)
- [Tupperware](https://engineering.fb.com/data-center-engineering/tupperware/).
