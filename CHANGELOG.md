# Changelog

## [Unreleased]

### Updated

- [Argo CD](applications/argo-cd/10.12.1/) `10.7.2` → `10.12.1` (Argo CD `v3.6.1`)
  - **The UI has moved from `/argo-cd` to `/mdr/argo-cd`.** The NKP dashboard link is updated automatically; update any bookmarks or CLI `--grpc-web-root-path` settings.
  - The dashboard entry now reports the correct chart version.
- [Dynatrace Operator](applications/dynatrace-operator/1.11.1/) `1.6.1` → `1.11.1`
  - **Now installs into the `dynatrace` namespace** instead of the workspace namespace, matching the upstream chart defaults and Dynatrace documentation. Create `DynaKube` resources there.
  - `DynaKube` API versions `v1beta1`–`v1beta4` are no longer served; use `v1beta5` or `v1beta6`.
  - The overview now links to the Dynatrace Kubernetes quickstart for next steps (creating a `DynaKube`).

> [!NOTE]
> This file was reconstructed retrospectively from git tags, so entries up to and including `0.2.0` list only which applications were added or updated.

## [0.2.0] - 2026-11-04

### Added

- [ECK Elasticsearch](applications/eck-elasticsearch/0.20.0/) `0.20.0`
- [ECK Kibana](applications/eck-kibana/0.20.0/) `0.20.0`
- [Open WebUI](applications/open-webui/18.0.0/) `18.0.0`
- [Valkey](applications/valkey/0.12.1/) `0.12.1`
- [Vault](applications/vault/0.34.1/) `0.34.1`

### Updated

- [GitLab](applications/gitlab/9.11.16/) `9.0.2` → `9.11.16`

## [0.1.0] - 2026-10-23

### Added

- [VictoriaMetrics Operator](applications/victoria-metrics-operator/0.68.1/) `0.68.1`
- [VictoriaMetrics Single](applications/vmsingle/0.1.4/) `0.1.4`

### Updated

- [MDR Demo](applications/mdr-demo/0.1.2/) `0.1.1` → `0.1.2`

## [0.0.8] - 2026-10-22

### Updated

- [Kasten K10](applications/k10/9.0.5-mdr.6/) `9.0.5-mdr.5` → `9.0.5-mdr.6`

## [0.0.7] - 2026-10-22

### Updated

- Kasten K10 `9.0.5-mdr.4` → `9.0.5-mdr.5`

## [0.0.6] - 2026-09-17

### Updated

- Kasten K10 `9.0.5-mdr.3` → `9.0.5-mdr.4`

## [0.0.5] - 2026-09-16

### Added

- [Arcadians](applications/arcadians/0.1.0/) `0.1.0`

## [0.0.4] - 2026-09-16

### Added

- Kasten K10 `9.0.5-mdr.3`

## [0.0.3] - 2026-09-04

### Added

- [Argo CD](applications/argo-cd/10.7.2/) `10.7.2`
- [Dynatrace Operator](applications/dynatrace-operator/1.6.1/) `1.6.1`
- [ECK Operator](applications/eck-operator/3.5.0/) `3.5.0`
- GitLab `9.0.2`

## [0.0.2] - 2026-09-04

### Added

- [Acme Issuers](applications/acme-issuers/0.4.1/) `0.4.1`

## [0.0.1] - 2026-09-04

Initial release.

### Added

- [MDR Common](applications/mdr-common/0.1.0/) `0.1.0`
- MDR Demo `0.1.1`
