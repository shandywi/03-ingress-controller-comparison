# WSL workstation setup

Project 03 is operated from Ubuntu on WSL2 so the local workflow matches common Linux-based DevOps environments.

## Installed tools

- `kubectl` from the Kubernetes v1.36 package repository, aligned to the K3s v1.36 server minor version.
- Helm from the official Helm Debian package repository.
- Supporting CLI utilities: `ca-certificates`, `curl`, `gnupg`, `git`, `jq`, and `bash-completion`.

## Kubeconfig handling

The Windows kubeconfig is copied to `~/.kube/config` inside WSL and protected with mode `0600`. The containing directory uses mode `0700`.

```bash
install -d -m 700 ~/.kube
cp /mnt/c/Users/<windows-user>/.kube/config ~/.kube/config
chmod 600 ~/.kube/config
```

The kubeconfig is an operator credential. It is never stored in this repository, included in evidence, or copied into a container image.

## Validation completed

- `kubectl config current-context`
- `kubectl version --short`
- a node-list request using custom columns

## Repository path from WSL

```bash
cd /mnt/c/Users/<windows-user>/<workspace>/projects/03-ingress-controller-comparison
```
