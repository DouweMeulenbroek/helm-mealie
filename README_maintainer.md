# This file is for the maintainer of this repository.

# Mealie Helm Chart Repository

This repository contains the Helm chart for [Mealie](https://github.com/mealie-recipes/mealie) and the workflow to automatically package and publish it to GitHub Pages as a Helm repository.

---

## Chart Structure
```
charts/
└── mealie/
├── Chart.yaml # Chart metadata
├── values.yaml # Configuration defaults
├── templates/ # Kubernetes manifests
├── Chart.lock # Optional: dependency lock file
├── README.md # Chart-specific readme (for helm show readme)
└── .helmignore # Files to exclude from packaging
.github/workflows/
└── publish-helm.yaml # CI/CD workflow

```

---

## Versioning

- **`version`**: Helm chart version, auto-bumped by the workflow based on commit messages:
  - `fix:` → patch bump  
  - `feat:` → minor bump  
  - `BREAKING CHANGE` → major bump  
- **`appVersion`**: upstream Mealie application version (Docker image). Must be manually updated when you change the image version or app version.

**Example:**  
- Updating Docker image from `v3.1.2` → `v3.1.3`:
  1. Update `appVersion: "v3.1.3"` in `Chart.yaml`.
  2. Commit with a message like `fix: update Docker image to v3.1.3`.
  3. Workflow will bump chart patch version and publish `.tgz` and `index.yaml`.

---

## Workflow Overview

1. Triggered on push to `main`.
2. Lints chart (`helm lint`).
3. Updates chart dependencies (`helm dependency update`), including PostgreSQL subchart.
4. Determines chart version bump based on commit messages.
5. Packages chart respecting `.helmignore`.
6. Pushes packaged `.tgz` and updated `index.yaml` to `gh-pages`.
7. Helm users can install/update via:

```bash
helm repo add mealie https://douwemeulenbroek.github.io/helm-mealie
helm repo update
helm install my-mealie mealie/mealie --version <chart-version>

``

# Cheat sheet
# Mealie Helm Chart - Quick Reference

## Installing / Updating Helm Chart
```bash
helm repo add mealie https://douwemeulenbroek.github.io/helm-mealie
helm repo update
helm install my-mealie mealie/mealie --version <chart-version>
helm upgrade my-mealie mealie/mealie -f values.yaml
``` 

