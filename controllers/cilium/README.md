# Cilium Ingress

**Status:** Planned  
**Focus:** Enable and operate Cilium's Envoy-backed ingress capability in the existing Cilium-based K3s cluster.

## Scope

- Record the current Cilium Helm release and health before changing networking behaviour.
- Enable ingress through a reviewed Cilium Helm upgrade, using the explicit `cilium` IngressClass.
- Route the same demo workload used in the other controller projects.
- Validate controller readiness, routing, rollback, and the absence of regressions in Cilium networking.

## Important safety boundary

Cilium is the cluster CNI, so this is a networking change rather than a standalone application install. We will back up the active Helm values, make one controlled change, check all Cilium components, and retain a documented rollback command before proceeding.

## Controller-switch rule

Cilium ingress has its own LoadBalancer IP and explicit `cilium` IngressClass. The validation evidence will distinguish Cilium ingress from the pre-existing Cilium networking and Hubble components.
