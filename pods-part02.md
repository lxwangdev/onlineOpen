# Using and Managing Pods (Part 02 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Using Pods

The following is an example of a Pod which consists of a container running the image `nginx:1.14.2`.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80
```

To create the Pod shown above, run the following command:

```shell
kubectl apply -f https://k8s.io/examples/pods/simple-pod.yaml
```

Pods are generally not created directly and are created using workload resources. See [Working with Pods](#working-with-pods) for more information on how Pods are used with workload resources.

### Workload resources for managing pods

Usually you don't need to create Pods directly, even singleton Pods. Instead, create them using workload resources such as [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) or [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/). If your Pods need to track state, consider the [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) resource.

Each Pod is meant to run a single instance of a given application. If you want to scale your application horizontally (to provide more overall resources by running more instances), you should use multiple Pods, one for each instance. In Kubernetes, this is typically referred to as *replication*. Replicated Pods are usually created and managed as a group by a workload resource and its [controller](https://kubernetes.io/docs/concepts/architecture/controller/).

See [Pods and controllers](#pods-and-controllers) for more information on how Kubernetes uses workload resources, and their controllers, to implement application scaling and auto-healing.

Pods natively provide two kinds of shared resources for their constituent containers: [networking](#pod-networking) and [storage](#pod-storage).

## Working with Pods

You'll rarely create individual Pods directly in Kubernetes—even singleton Pods. This is because Pods are designed as relatively ephemeral, disposable entities. When a Pod gets created (directly by you, or indirectly by a [controller](https://kubernetes.io/docs/concepts/architecture/controller/)), the new Pod is scheduled to run on a [Node](https://kubernetes.io/docs/concepts/architecture/nodes/) in your cluster. The Pod remains on that node until the Pod finishes execution, the Pod object is deleted, the Pod is *evicted* for lack of resources, or the node fails.

> **Note:** Restarting a container in a Pod should not be confused with restarting a Pod. A Pod is not a process, but an environment for running container(s). A Pod persists until it is deleted.

The name of a Pod must be a valid [DNS subdomain](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-subdomain-names) value, but this can produce unexpected results for the Pod hostname. For best compatibility, the name should follow the more restrictive rules for a [DNS label](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/#dns-label-names).
