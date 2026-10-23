# Caddy Ingress Controller

**Status:** Planned  
**Focus:** Operate Caddy as the only active Kubernetes ingress controller and compare its operational model with Traefik, NGINX, and Cilium.

## Scope

- Install Caddy's Kubernetes ingress controller with an explicit `caddy` IngressClass.
- Expose and validate the standard demo application.
- Record resource usage, route behaviour, troubleshooting steps, and teardown evidence.
- Integrate certificate Secrets issued by the cert-manager profile in this repository when TLS is tested.

## Certificate ownership decision

For the portfolio comparison, cert-manager owns certificate issuance and renewal. Caddy consumes the resulting Kubernetes TLS Secret. We will not enable a second automatic ACME lifecycle in Caddy, because two systems attempting to manage the same certificate complicate renewal and incident diagnosis.

## Controller-switch rule

Caddy receives its own LoadBalancer IP and processes only routes assigned to the explicit `caddy` IngressClass.
