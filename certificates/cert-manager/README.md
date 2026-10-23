# cert-manager TLS Automation

**Status:** Planned  
**Focus:** Manage TLS certificate issuance, renewal, and troubleshooting separately from ingress-controller installation.

## Scope

- Install cert-manager with version-pinned Helm values and resource limits.
- Define safe issuer patterns and document the difference between staging and production ACME endpoints.
- Demonstrate a Certificate becoming a Kubernetes TLS Secret, renewal observation, and failure troubleshooting.
- Configure the HTTP-01 solver for whichever single ingress controller is active at the time.

## Why it is separate

Ingress controllers route traffic; cert-manager owns certificate lifecycle. Separating them makes it clear that a controller can be replaced without changing the certificate-management architecture. When the active controller changes, the Issuer solver configuration is reviewed and updated as part of the migration.

## Safety boundary

No real domain name, e-mail address, private key, certificate, token, or IP address will be committed. A real ACME issuance test will happen only after DNS ownership and public-routing requirements have been confirmed.
