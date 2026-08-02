# Security and Resource Limits (Part 08 of 10)

Source: https://kubernetes.io/docs/concepts/workloads/pods/
License: CC BY 4.0 (Kubernetes documentation)
Retrieved: 2026-08-02

---

## Pod security settings

To set security constraints on Pods and containers, you use the `securityContext` field in the Pod specification. This field gives you granular control over what a Pod or individual containers can do. See [Advanced Pod Configuration](https://kubernetes.io/docs/concepts/workloads/pods/advanced-pod-config/) for more details.

For basic security configuration, you should meet the Baseline Pod security standard and run containers as non-root. You can set simple security contexts:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: security-context-demo
spec:
  securityContext:
    runAsUser: 1000
    runAsGroup: 3000
    fsGroup: 2000
  containers:
  - name: sec-ctx-demo
    image: busybox
    command: ["sh", "-c", "sleep 1h"]
```

For advanced security context configuration including capabilities, seccomp profiles, and detailed security options, see the [security concepts](https://kubernetes.io/docs/concepts/security/) section.

- To learn about kernel-level security constraints that you can use, see [Linux kernel security constraints for Pods and containers](https://kubernetes.io/docs/concepts/security/linux-kernel-security-constraints/).
- To learn more about the Pod security context, see [Configure a Security Context for a Pod or Container](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/).

## Resource requests and limits

When you specify a Pod, you can optionally specify how much of each resource a container needs. The most common resources to specify are CPU and memory (RAM).

When you specify the resource *request* for containers in a Pod, the kube-scheduler uses this information to decide which node to place the Pod on. When you specify a resource *limit* for a container, the kubelet enforces those limits so that the running container is not allowed to use more of that resource than the limit you set.

CPU limits are enforced by CPU throttling. When a container approaches its CPU limit, the kernel restricts its access to CPU. Memory limits are enforced by the kernel with out-of-memory (OOM) kills when a container exceeds its limit.

> **Note:** Setting CPU limits involves a trade-off. CPU limits help prevent noisy neighbor problems where a single workload starves others on the same node. This is especially important in multi-tenant environments. However, CPU limits can cause throttling even when the node has spare CPU capacity, potentially degrading latency-sensitive workload performance. Whether to set CPU limits depends on your environment, workload characteristics, and isolation requirements.

For details on resource units, enforcement behavior, and configuration examples, see [Resource Management for Pods and Containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/).
