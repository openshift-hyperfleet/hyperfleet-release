# HyperFleet MVP Release - Release Notes

**Version:** v0.2.0  
**Release Date:** April 1, 2026

> **IMPORTANT**: This MVP release is for exploration and evaluation only. Do not use in production environments.

## Table of Contents

- [Overview](#overview)
- [Component Versions](#component-versions)
- [Known Issues](#known-issues)
- [Getting Started](#getting-started)
- [Additional Notes](#additional-notes)
- [Support](#support)

## Overview

This is the **first official release** of HyperFleet. The MVP delivers core cluster and node pool lifecycle management with event-driven architecture, policy-based automation, and complete observability.

**HyperFleet API**
- RESTful API for cluster and nodepool lifecycle management (Create, Read, List operations)
- Advanced filtering and querying with Tree Search Language (TSL) and label-based searches
- Kubernetes-style status conditions with adapter status reporting
- PostgreSQL backend with JSONB for flexible spec storage
- Prometheus metrics, structured logging, and OpenTelemetry integration
- Liveness and readiness probes with database connectivity checks

**HyperFleet Sentinel**
- Periodic resource polling with configurable intervals
- Label-based resource filtering for horizontal scaling
- CEL-based decision engine for determining when to publish reconciliation events
- CloudEvents publishing to message brokers (Pub/Sub, RabbitMQ)
- Prometheus metrics, structured logging, and OpenTelemetry integration
- Liveness and readiness probes with broker connectivity and poll staleness checks

**HyperFleet Adapter**
- Dual transporter support: Kubernetes (in-cluster with Create, Update, Apply operations) and Maestro (remote cluster with Create, Get, Apply operations via ManifestWork)
- Four-phase execution pipeline: Parameters → Preconditions → Resources → Post-Actions
- CEL expressions for precondition evaluation, field capture, and status payload generation
- Template rendering with Go templates for dynamic resource generation
- Status reporting back to external APIs with configurable payload building
- Prometheus metrics, structured logging, and OpenTelemetry integration
- Liveness and readiness probes with configuration and broker subscription checks

## Component Versions

All components are released together at v0.2.0:

| Component | Version | GitHub Release Notes                                                                                  | Container Image | SHA256 Digest |
|-----------|---------|----------------------------------------------------------------------------------------------------------|-----------------|---------------|
| **HyperFleet API** | v0.2.0 | [View Detailed Release Notes](https://github.com/openshift-hyperfleet/hyperfleet-api/releases/tag/v0.2.0) | `quay.io/openshift-hyperfleet/hyperfleet-api:v0.2.0` | `df91916b3e1d08f873e023d3e86e574086f16fa88dafb04c158db8fb24007e6b` |
| **HyperFleet Sentinel** | v0.2.0 | [View Detailed Release Notes](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/releases/tag/v0.2.0)        | `quay.io/openshift-hyperfleet/hyperfleet-sentinel:v0.2.0` | `0f400d1bef0af3c9285dfe349e6e436e9dbb5cc16c3ad0649299b216c6952ff0` |
| **HyperFleet Adapter** | v0.2.0 | [View Detailed Release Notes](https://github.com/openshift-hyperfleet/hyperfleet-adapter/releases/tag/v0.2.0) | `quay.io/openshift-hyperfleet/hyperfleet-adapter:v0.2.0` | `dc5af911b16b6b720ab38b2a6c5128377b5b3fdd648e85bf5d24a2e91940b902` |

## Known Issues

### CVE: gRPC-Go authorization bypass via missing leading slash in :path

- **Severity**: CRITICAL
- **Description**: Authorization bypass vulnerability in gRPC-Go (google.golang.org/grpc v1.79.2) where requests with malformed :path headers can bypass authorization interceptors. Affects HyperFleet Sentinel and Adapter components
- **Impact**: Customers who play with the image should not expose the service to the internet. Port-forward can make it safe. We will fix it in 0.2.1. Right now there is no production environment using 0.2.0 images
- **Tracking**: [HYPERFLEET-846](https://redhat.atlassian.net/browse/HYPERFLEET-846)

### CVE: glibc iconv() assertion failure DoS via IBM1390/IBM1399 character sets

- **Severity**: HIGH
- **Description**: CVE vulnerability in glibc iconv() that could lead to denial of service via IBM1390/IBM1399 character sets
- **Impact**: Customers who play with the image should not expose the service to the internet. Port-forward can make it safe. We will fix it in 0.2.1. Right now there is no production environment using 0.2.0 images
- **Tracking**: [HYPERFLEET-844](https://redhat.atlassian.net/browse/HYPERFLEET-844)

### Sentinel Memory/CPU Usage at Very Large Scale

- **Severity**: LOW
- **Description**: In-memory CEL evaluation of full resource set may increase resource usage at very large scale
- **Workaround**: Use `resource_selector` labels for horizontal scaling across multiple instances
- **Reference**: [Multi-instance deployment guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/multi-instance-deployment.md)
- **Tracking**: [HYPERFLEET-805](https://redhat.atlassian.net/browse/HYPERFLEET-805)

### Version Command/Output Inconsistency

- **Severity**: LOW
- **Description**: Inconsistent version command and output format across components
- **Impact**: None. Functionality is not affected
- **Tracking**: [HYPERFLEET-819](https://redhat.atlassian.net/browse/HYPERFLEET-819)

## Getting Started

### Operator Guides

- **HyperFleet API**: [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
- **HyperFleet Sentinel**: [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
- **HyperFleet Adapter**: [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

### Helm Charts

Helm charts are available in each component repository under the `charts/` directory. See the operator guides above for detailed configuration options.

## Additional Notes

- **Not Production-Ready**: Security scanning and compliance validation will be added in future releases
- **No Upgrade Path**: As the first release, there are no upgrade procedures (future releases will include upgrade guides)
- **Konflux Integration**: Planned for future releases to provide security compliance

## Support

- **Issues**: Report in JIRA HYPERFLEET project or in the respective component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: See [operator guides](#operator-guides) above
