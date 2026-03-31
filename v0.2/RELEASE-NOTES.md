# HyperFleet MVP Release - Release Notes

**Version:** v0.2.0
**Release Date:** April 1, 2026

> **IMPORTANT**: This MVP release is for exploration and evaluation only. Do not use in production environments.

## Table of Contents

- [Overview](#overview)
- [What's Included](#whats-included)
- [Key Features](#key-features)
- [Known Issues](#known-issues)
- [Component Versions](#component-versions)
- [Getting Started](#getting-started)
- [Documentation](#documentation)
- [Additional Notes](#additional-notes)
- [Support](#support)

## Overview

This is the **first official release** of HyperFleet. The MVP delivers core cluster and node pool lifecycle management with event-driven architecture, policy-based automation, and complete observability.

## What's Included

### HyperFleet API
- Create, Read, List operations for clusters and node pools
- PostgreSQL backend with connection pooling and migration support
- Tree Search Language (TSL) for advanced filtering by labels and status conditions
- Kubernetes-style status aggregation with condition queries
- OpenAPI/Swagger documentation and health checks

### HyperFleet Sentinel
- CEL-based decision engine for flexible reconciliation logic
- CloudEvents publishing to message brokers (Pub/Sub, RabbitMQ)
- OpenTelemetry distributed tracing with trace context propagation
- Prometheus metrics with staleness detection
- Horizontal scaling via label-based resource selectors

### HyperFleet Adapter
- Kubernetes Transporter for local cluster resource management
- Maestro Transporter for remote cluster management via OCM ManifestWork
- CEL-based precondition system with time and API call checks
- Four-phase execution pipeline (Parameters → Preconditions → Resources → Post-Actions)
- Template rendering and status reporting

## Key Features

- **Event-Driven Architecture**: Automated resource monitoring and CloudEvents publishing
- **Policy-Based Automation**: CEL expressions for decision making, preconditions, and transformations
- **Dual Transporter Support**: Flexible deployment to local or remote clusters
- **Complete Observability**: OpenTelemetry tracing, Prometheus metrics, structured logging
- **Horizontal Scaling**: Multi-instance deployment with resource sharding
- **Helm Deployment**: Production-ready charts following Helm conventions

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

## Component Versions

All components are released together at v0.2.0:

| Component | Version |
|-----------|---------|
| hyperfleet-api | v0.2.0 |
| hyperfleet-sentinel | v0.2.0 |
| hyperfleet-adapter | v0.2.0 |

## Getting Started

### Operator Guides

- **HyperFleet API**: [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
- **HyperFleet Sentinel**: [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
- **HyperFleet Adapter**: [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

### Container Images

| Component | Image | SHA256 Digest |
|-----------|-------|---------------|
| hyperfleet-api | quay.io/openshift-hyperfleet/hyperfleet-api:v0.2.0 | `df91916b3e1d08f873e023d3e86e574086f16fa88dafb04c158db8fb24007e6b` |
| hyperfleet-sentinel | quay.io/openshift-hyperfleet/hyperfleet-sentinel:v0.2.0 | `0f400d1bef0af3c9285dfe349e6e436e9dbb5cc16c3ad0649299b216c6952ff0` |
| hyperfleet-adapter | quay.io/openshift-hyperfleet/hyperfleet-adapter:v0.2.0 | `dc5af911b16b6b720ab38b2a6c5128377b5b3fdd648e85bf5d24a2e91940b902` |

### Helm Charts

Helm charts are available in each component repository under the `charts/` directory. See the [operator guides](#operator-guides) for detailed configuration options.

## Documentation

- **[Change Log](./CHANGELOG.md)** - Detailed technical changes for all components

## Additional Notes

- **Not Production-Ready**: Security scanning and compliance validation will be added in future releases
- **No Upgrade Path**: As the first release, there are no upgrade procedures (future releases will include upgrade guides)
- **Konflux Integration**: Planned for future releases to provide security compliance

## Support

- **Issues**: Report in JIRA HYPERFLEET project or in the respective component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: See [operator guides](#operator-guides) above
