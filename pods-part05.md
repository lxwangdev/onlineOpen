# Pod Updates and Subresources (Part 05 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Pod update and replacement

As mentioned in the previous section, when the Pod template for a workload resource is changed, the controller creates new Pods based on the updated template instead of updating or patching the existing Pods.

Kubernetes doesn't prevent you from managing Pods directly. It is possible to update some fields of a running Pod, in place. However, Pod update operations like [`patch`](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.36/#patch-pod-v1-core), and [`replace`](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.36/#replace-pod-v1-core) have some limitations:

- Most of the metadata about a Pod is immutable. For example, you cannot change the `namespace`, `name`, `uid`, or `creationTimestamp` fields.
- If the `metadata.deletionTimestamp` is set, no new entry can be added to the `metadata.finalizers` list.
- Pod updates may not change fields other than `spec.containers[*].image`, `spec.initContainers[*].image`, `spec.activeDeadlineSeconds`, `spec.terminationGracePeriodSeconds`, `spec.tolerations` or `spec.schedulingGates`. For `spec.tolerations`, you can only add new entries.
- When updating the `spec.activeDeadlineSeconds` field, two types of updates are allowed:
  1. setting the unassigned field to a positive number;
  2. updating the field from a positive number to a smaller, non-negative number.

### Pod subresources

The above update rules apply to regular pod updates, but other pod fields can be updated through *subresources*.

- **Resize:** The `resize` subresource allows container resources (`spec.containers[*].resources`) to be updated. See [Resize Container Resources](https://kubernetes.io/docs/tasks/configure-pod-container/resize-container-resources/) for more details.
- **Ephemeral Containers:** The `ephemeralContainers` subresource allows [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) to be added to a Pod. See [Ephemeral Containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) for more details.
- **Status:** The `status` subresource allows the pod status to be updated. This is typically only used by the Kubelet and other system controllers.
- **Binding:** The `binding` subresource allows setting the pod's `spec.nodeName` via a `Binding` request. This is typically only used by the [scheduler](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/).
