# HyperFleet Release 0.2.1 - Release Notes

**Version:** v0.2.1  
**Release Date:** April 14, 2026  
**Type:** Z-stream (patch release)

> **IMPORTANT**: This MVP release is for exploration and evaluation only. Do not use in production environments.

## Table of Contents

- [Overview](#overview)
- [Component Versions](#component-versions)
- [Bug Fixes](#bug-fixes)
- [Known Issues](#known-issues)
- [Upgrade Notes](#upgrade-notes)
- [Support](#support)

## Overview

This is a **z-stream patch release** for HyperFleet 0.2, addressing critical CVE fixes, a race condition in the API, and configuration improvements for Sentinel and E2E testing.

## Component Versions

| Component | Version | Container Image | SHA256 Digest |
|-----------|---------|-----------------|---------------|
| **HyperFleet API** | v0.2.1 | `quay.io/openshift-hyperfleet/hyperfleet-api:v0.2.1` | `c2957324b525fbd7d5ec776f4a395783599e6a878fb3f5993edc06371f1579bd` |
| **HyperFleet Sentinel** | v0.2.1 | `quay.io/openshift-hyperfleet/hyperfleet-sentinel:v0.2.1` | `973b9ecd23f5515df4d1da4431f0403178e9e9739b99cb5559e1326f43a242c8` |
| **HyperFleet Adapter** | v0.2.1 | `quay.io/openshift-hyperfleet/hyperfleet-adapter:v0.2.1` | `cbc27c94e77cdd09693305ef89d475a96089dcbfce1d3ff88634362da25b2ad8` |

## Bug Fixes

### CRITICAL: gRPC-Go authorization bypass (CVE-2026-33186)

- **Component:** API, Sentinel, Adapter
- **Description:** Bumped `google.golang.org/grpc` from v1.79.2 to v1.79.3 to fix an authorization bypass vulnerability where requests with malformed `:path` headers could bypass authorization interceptors.
- **Tracking:** [HYPERFLEET-846](https://redhat.atlassian.net/browse/HYPERFLEET-846), [HYPERFLEET-762](https://redhat.atlassian.net/browse/HYPERFLEET-762)

### Advisory lock race condition

- **Component:** API
- **Description:** Fixed a race condition where a database transaction could be created before the advisory lock was acquired, potentially leading to inconsistent state under concurrent writes.
- **Tracking:** [HYPERFLEET-875](https://redhat.atlassian.net/browse/HYPERFLEET-875)

### Sentinel Helm chart values updated for new config schema

- **Component:** Sentinel (Helm chart)
- **Description:** Updated the Sentinel Helm chart values in `hyperfleet-infra` to align with the new standard configuration schema introduced in release 0.2.
- **Tracking:** [HYPERFLEET-866](https://redhat.atlassian.net/browse/HYPERFLEET-866)

### E2E sentinel broker config fix

- **Component:** E2E
- **Description:** Fixed the sentinel broker configuration in E2E tests to use root-level config keys, resolving test failures on the release-0.2 branch.
- **Tracking:** [HYPERFLEET-936](https://redhat.atlassian.net/browse/HYPERFLEET-936)

### Version command/output standardized

- **Component:** Sentinel, Adapter
- **Description:** Standardized the version command output format across all HyperFleet components for consistency.
- **Tracking:** [HYPERFLEET-819](https://redhat.atlassian.net/browse/HYPERFLEET-819)

### ManifestWork Go template support for lists and conditionals

- **Component:** Adapter
- **Description:** Fixed ManifestWork manifest templates to support Go template lists and conditionals (e.g., `range`, `if`) when defined with adapters.
- **Tracking:** [HYPERFLEET-864](https://redhat.atlassian.net/browse/HYPERFLEET-864)

## Known Issues

| Issue Description | Severity | Workaround (if applicable) | Internal Tracking Link |
|---|---|---|---|
| Sentinel fetches the full resource set from the API and evaluates each resource in-memory via CEL. At a very large scale this may increase memory/CPU usage. | LOW | Use `resource_selector` labels to shard across multiple Sentinel instances (horizontal scaling). Docs: [multi-instance-deployment.md](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/multi-instance-deployment.md) | [HYPERFLEET-805](https://redhat.atlassian.net/browse/HYPERFLEET-805) |
| CVE: glibc iconv() assertion failure DoS via IBM1390/IBM1399 character sets | HIGH | Do not expose the service directly to the internet. Use port-forward or internal networking for access. | [HYPERFLEET-844](https://redhat.atlassian.net/browse/HYPERFLEET-844) |
| Pagination default pageSize mismatch between OpenAPI spec (20) and code implementation (100) | LOW | Explicitly pass `size` parameter in API requests to ensure consistent pagination behavior. | [HYPERFLEET-841](https://redhat.atlassian.net/browse/HYPERFLEET-841) |

## Upgrade Notes

This is a drop-in replacement for v0.2.0. No configuration changes or migrations are required.

To upgrade, update the image tags in your Helm values:

```yaml
image:
  tag: v0.2.1
```

## Support

- **Issues**: Report in JIRA HYPERFLEET project or in the respective component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: See [operator guides](RELEASE-NOTES-v0.2.md#getting-started)
