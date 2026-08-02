# Multi-container Pods and Probes (Part 06 of 06)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Pods with multiple containers

Pods are designed to support multiple cooperating processes (as containers) that form a cohesive unit of service. The containers in a Pod are automatically co-located and co-scheduled on the same physical or virtual machine in the cluster. The containers can share resources and dependencies, communicate with one another, and coordinate when and how they are terminated.

Pods in a Kubernetes cluster are used in two main ways:

- **Pods that run a single container**. The "one-container-per-Pod" model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container; Kubernetes manages Pods rather than managing the containers directly.
- **Pods that run multiple containers that need to work together**. A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers form a single cohesive unit of service—for example, one container serving data stored in a shared volume to the public, while a separate [sidecar container](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) refreshes or updates those files. The Pod wraps these containers, storage resources, and an ephemeral network identity together as a single unit.

For example, you might have a container that acts as a web server for files in a shared volume, and a separate [sidecar container](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) that updates those files from a remote source, as in the following diagram:

![Pod creation diagram](https://kubernetes.io/images/docs/pod.svg)

Some Pods have [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) as well as [app containers](https://kubernetes.io/docs/reference/glossary/?all=true#term-app-container). By default, init containers run and complete before the app containers are started.

You can also have [sidecar containers](https://kubernetes.io/docs/concepts/workloads/pods/sidecar-containers/) that provide auxiliary services to the main application Pod (for example: a service mesh).

> **FEATURE STATE: `Kubernetes v1.33 [stable]` (enabled by default) *(Feature Gate: SidecarContainers)*

Enabled by default, the `SidecarContainers` [feature gate](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/) allows you to specify `restartPolicy: Always` for init containers. Setting the `Always` restart policy ensures that the containers where you set it are treated as *sidecars* that are kept running during the entire lifetime of the Pod. Containers that you explicitly define as sidecar containers start up before the main application Pod and remain running until the Pod is shut down.

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
