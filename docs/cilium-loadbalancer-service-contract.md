# Cilium LoadBalancer exposure contract

Every ingress-controller Service in this project must use the following Cilium exposure contract. This enables a consistent, reviewable way to request an address from the homelab LoadBalancer IP pool and advertise it through Cilium L2 announcements.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: <controller-service-name>
  namespace: <controller-namespace>
  labels:
    homelab.io/lb: "true"
spec:
  type: LoadBalancer
  loadBalancerClass: io.cilium/l2-announcer
  externalTrafficPolicy: Cluster
  selector:
    <controller-pod-label>: <controller-pod-value>
  ports:
    - name: web
      protocol: TCP
      port: 80
      targetPort: web
    - name: websecure
      protocol: TCP
      port: 443
      targetPort: websecure
```

## Required fields

| Field | Required value | Reason |
| --- | --- | --- |
| `metadata.labels.homelab.io/lb` | `"true"` | Matches the selector defined on the homelab Cilium LoadBalancer IP pool. |
| `spec.type` | `LoadBalancer` | Requests a dedicated address for the controller Service. |
| `spec.loadBalancerClass` | `io.cilium/l2-announcer` | Delegates advertisement of the allocated address to the Cilium L2 announcer. |
| `spec.externalTrafficPolicy` | `Cluster` | Allows any announcing node to forward traffic to ready controller Pods across the cluster. |

## Consequences for the comparison

- Traefik, F5 NGINX, Cilium ingress, and Caddy may all expose ports `80` and `443` concurrently because each Service receives a different LoadBalancer IP.
- The label is mandatory. Without it, the `homelab-lb-pool` selector will not allocate an address to the Service.
- The Service selector and target-port names are controller-specific. Do not copy the placeholder selector or port names blindly; verify them against the installed chart's Pod labels and container ports.
- Evidence must redact every allocated address as `172.16.0.0/24` before it is committed.

## Pre-flight validation

Before installing a controller, confirm the pool exists and is enabled without printing its address range:

```powershell
kubectl get ciliumloadbalancerippools `
  -o custom-columns=NAME:.metadata.name,DISABLED:.spec.disabled
```

After installation, verify the controller Service has received an address locally. Do not commit the command output until the address has been redacted.
