# MDR NKP Catalog Apps

![Platform](https://img.shields.io/badge/platform-Nutanix_NKP-blue)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/markround/nkp-catalog/catalog.yml)
![GitHub branch check runs](https://img.shields.io/github/check-runs/markround/nkp-catalog/main)

## Overview

My personal set of catalog applications for the Nutanix NKP platform. It serves two purposes:

* providing applications I need for demos that aren't in the stock NKP catalog, and
* acting as an example of a fully automated catalog delivery pipeline which can validate, bundle,
  and push to an OCI registry on every commit.

> [!IMPORTANT]
> Nothing in here is "official" and it is provided without any warranty. It is not supported,
> endorsed by, or otherwise connected to Nutanix or any of the software products packaged within.
> Use it at your own risk!

## What's in the catalog

| Application | Version | Category | Upstream chart |
| --- | --- | --- | --- |
| [Acme Issuers](applications/acme-issuers/0.4.1/) | 0.4.1 | tools | `oci://ghcr.io/markround/helm/acme-issuers` |
| [Arcadians](applications/arcadians/0.1.0/) | 0.1.0 | games | _(plain manifests)_ |
| [Argo CD](applications/argo-cd/10.7.2/) | 10.7.2 | tools | `oci://ghcr.io/argoproj/argo-helm/argo-cd` |
| [Dynatrace Operator](applications/dynatrace-operator/1.6.1/) | 1.6.1 | observability | `oci://public.ecr.aws/dynatrace/dynatrace-operator` |
| [ECK Operator](applications/eck-operator/3.5.0/) | 3.5.0 | logging | `https://helm.elastic.co` |
| [GitLab](applications/gitlab/9.0.2/) | 9.0.2 | general | `https://charts.gitlab.io` |
| [Kasten K10](applications/k10/9.0.5-mdr.4/) | 9.0.5-mdr.4 | backup | `https://charts.kasten.io/` |
| [MDR Common](applications/mdr-common/0.1.0/) | 0.1.0 | general | _(plain manifests)_ |
| [MDR Demo](applications/mdr-demo/0.1.1/) | 0.1.1 | general | _(plain manifests)_ |

All applications are workspace-scoped and require a NKP Ultimate licence (as you can't add custom catalogs on anything less than NKP Ultimate).

A few notes on the less obvious/non-upstream project entries:

* **ACME Issusers** - my own [Helm chart](https://github.com/markround/acme-issuers) to make deploying ACME-based ClusterIssuers across clusters simple.
* **Arcadians** - a retro Galaxian-style game ([original](https://github.com/jdtate101/Arcadians) by James Tate, [my fork](https://github.com/markround/Arcadians)) packaged as a stateful demo app: a HTML5 frontend, a FastAPI backend and a PostgreSQL database for persistent high scores. Used to demo backup/restore and migration of stateful workloads. Always deploys into the `retro-game` namespace.
* **MDR Common** - a demo app to show how you could deploy shared manifests you want available on all clusters (currently just a demo CA certificate secret).
* **MDR Demo** - a trivial Nginx app used to show ingress and dashboard capabilities. It declares a dependency on `mdr-common`.

## Installation

Requires **NKP Ultimate**. Currently only tested on **NKP 2.18**.

Deploying into `kommander-workspace` makes the catalog available across all workspaces and
projects.

`main` is the latest code. It should work, but is not 100% validated:

```bash
nkp create catalog-collection \
  --url oci://ghcr.io/markround/nkp-catalog/catalog/collection \
  --tag main \
  --workspace kommander-workspace
```

Or pick a released tag with (theoretically!) less chance of breakage:

```bash
nkp create catalog-collection \
  --url oci://ghcr.io/markround/nkp-catalog/catalog/collection \
  --tag 0.0.5 \
  --workspace kommander-workspace
```

Once the collection is registered, the applications show up in the NKP UI under
**Applications → Catalog Applications**, where they can be enabled per workspace. Add `--dry-run`
to either command to print the generated resources instead of applying them.

## Upgrading

If you have deployed a tag, you can simply patch the resource to point to a newer tag on the 
management cluster. For example:

```bash
kubectl patch \
  --type merge \
  -n kommander \
  ocirepository catalog-collection \
  --patch '{"spec": {"ref":{"tag":"0.0.5"}}}'
```

