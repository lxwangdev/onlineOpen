# Static and Multi-container Pods (Part 09 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Static Pods

*Static Pods* are managed directly by the kubelet daemon on a specific node, without the [API server](https://kubernetes.io/docs/concepts/architecture/#kube-apiserver) observing them. Whereas most Pods are managed by the control plane (for example, a [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)), for static Pods, the kubelet directly supervises each static Pod (and restarts it if it fails).

Static Pods are always bound to one [Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet) on a specific node. The main use for static Pods is to run a self-hosted control plane: in other words, using the kubelet to supervise the individual [control plane components](https://kubernetes.io/docs/concepts/architecture/#control-plane-components).

For details, see [Static Pods](https://kubernetes.io/docs/concepts/workloads/pods/static-pods/).

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
