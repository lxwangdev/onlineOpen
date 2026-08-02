# Storage and Networking (Part 07 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Resource sharing and communication

Pods enable data sharing and communication among their constituent containers.

### Storage in Pods

A Pod can specify a set of shared storage [volumes](https://kubernetes.io/docs/concepts/storage/volumes/). All containers in the Pod can access the shared volumes, allowing those containers to share data. Volumes also allow persistent data in a Pod to survive in case one of the containers within needs to be restarted. See [Storage](https://kubernetes.io/docs/concepts/storage/) for more information on how Kubernetes implements shared storage and makes it available to Pods.

### Pod networking

Each Pod is assigned a unique IP address for each address family. Every container in a Pod shares the network namespace, including the IP address and network ports. Inside a Pod (and **only** then), the containers that belong to the Pod can communicate with one another using `localhost`. When containers in a Pod communicate with entities *outside the Pod*, they must coordinate how they use the shared network resources (such as ports). Within a Pod, containers share an IP address and port space, and can find each other via `localhost`. The containers in a Pod can also communicate with each other using standard inter-process communications like SystemV semaphores or POSIX shared memory. Containers in different Pods have distinct IP addresses and can not communicate by OS-level IPC without special configuration. Containers that want to interact with a container running in a different Pod can use IP networking to communicate.

Containers within the Pod see the system hostname as being the same as the configured `name` for the Pod. There's more about this in the [networking](https://kubernetes.io/docs/concepts/cluster-administration/networking/) section.
