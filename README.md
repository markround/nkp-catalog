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
| [Argo CD](applications/argo-cd/10.12.1/) | 10.12.1 | tools | `oci://ghcr.io/argoproj/argo-helm/argo-cd` |
| [Dynatrace Operator](applications/dynatrace-operator/1.11.1/) | 1.11.1 | observability | `oci://public.ecr.aws/dynatrace/dynatrace-operator` |
| [ECK Elasticsearch](applications/eck-elasticsearch/0.20.0/) | 0.20.0 | observability | `https://helm.elastic.co` |
| [ECK Kibana](applications/eck-kibana/0.20.0/) | 0.20.0 | observability | `https://helm.elastic.co` |
| [ECK Operator](applications/eck-operator/3.5.0/) | 3.5.0 | observability | `https://helm.elastic.co` |
| [GitLab](applications/gitlab/9.11.16/) | 9.11.16 | general | `oci://registry.gitlab.com/charts/charts.gitlab.io/release/gitlab` |
| [Kasten K10](applications/k10/9.0.5-mdr.6/) | 9.0.5-mdr.6 | backup | `https://charts.kasten.io/` |
| [MDR Common](applications/mdr-common/0.1.0/) | 0.1.0 | general | _(plain manifests)_ |
| [MDR Demo](applications/mdr-demo/0.1.2/) | 0.1.2 | general | _(plain manifests)_ |
| [Open WebUI](applications/open-webui/18.0.0-mdr.1/) | 18.0.0-mdr.1 | inferencing-and-serving | `oci://ghcr.io/open-webui/helm-charts/open-webui` |
| [Valkey](applications/valkey/0.12.1/) | 0.12.1 | general | `oci://ghcr.io/valkey-io/valkey-helm/valkey` |
| [Vault](applications/vault/0.34.1/) | 0.34.1 | security | `https://helm.releases.hashicorp.com` |
| [VictoriaMetrics Operator](applications/victoria-metrics-operator/0.68.1/) | 0.68.1 | observability | `oci://ghcr.io/victoriametrics/helm-charts/victoria-metrics-operator` |
| [VictoriaMetrics Single](applications/vmsingle/0.1.4/) | 0.1.4 | observability | _(plain manifests)_ |
| [Whereabouts](applications/whereabouts/0.9.4/) | 0.9.4 | networking | `oci://ghcr.io/k8snetworkplumbingwg/whereabouts-chart` |

All applications are workspace-scoped and require a NKP Ultimate licence (as you can't add custom catalogs on anything less than NKP Ultimate).

> [!NOTE]
> The version number reported is typically the version of the upstream Helm chart, not the bundled application.

A few notes on the less obvious/non-upstream project entries:

* **ACME Issusers** - my own [Helm chart](https://github.com/markround/acme-issuers) to make deploying ACME-based ClusterIssuers across clusters simple.
* **Arcadians** - a retro Galaxian-style game ([original](https://github.com/jdtate101/Arcadians) by James Tate, [my fork](https://github.com/markround/Arcadians)) packaged as a stateful demo app: a HTML5 frontend, a FastAPI backend and a PostgreSQL database for persistent high scores. Used to demo backup/restore and migration of stateful workloads. Always deploys into the `retro-game` namespace.
* **ECK Elasticsearch** - a single-node demo Elasticsearch cluster (based on the ECK quickstart) deployed with Elastic's `eck-elasticsearch` Helm chart, so node count, resources, storage etc. can be customised through Helm values. It declares a dependency on `eck-operator`.
* **ECK Kibana** - a single Kibana instance deployed with Elastic's `eck-kibana` Helm chart, connected to the ECK Elasticsearch cluster. It is served under `/mdr/kibana` through the NKP Traefik ingress, with a link on the NKP dashboard. It declares dependencies on `eck-operator` and `eck-elasticsearch`.
* **MDR Common** - a demo app to show how you could deploy shared manifests you want available on all clusters (currently just a demo CA certificate secret).
* **MDR Demo** - a trivial Nginx app used to show ingress and dashboard capabilities. It declares a dependency on `mdr-common`.
* **VictoriaMetrics Single** - a single-node VictoriaMetrics instance (a `VMSingle` resource) deployed into the workspace namespace. It declares a dependency on `victoria-metrics-operator`.

## Installation

Requires **NKP Ultimate**. Currently only tested on **NKP >= 2.18**.

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
  --tag 0.2.0 \
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
  --patch '{"spec": {"ref":{"tag":"0.2.0"}}}'
```

