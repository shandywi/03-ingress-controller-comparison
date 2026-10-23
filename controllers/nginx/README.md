# F5 NGINX Ingress Controller

**Status:** Planned  
**Focus:** Evaluate the maintained F5 NGINX Ingress Controller as the only active ingress controller in the lab.

## Scope

- Install the maintained F5 controller (`nginx/kubernetes-ingress`), not the retired community `kubernetes/ingress-nginx` project.
- Define an explicit `nginx` IngressClass and route a demo application through it.
- Apply resource-aware Helm values, validate routing, and capture redacted evidence.
- Document a complete teardown before the next controller is evaluated.

## Controller-switch rule

This controller is deployed with its own LoadBalancer IP and explicit `nginx` IngressClass. It must not be made the default class, so it processes only its deliberately classed routes.

## Reviewer outcome

A reviewer will be able to reproduce the installation, see a class-specific route working, understand why the maintained F5 implementation was chosen, and follow the rollback runbook.
