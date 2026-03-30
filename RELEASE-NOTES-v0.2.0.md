# HyperFleet MVP Release - Release Notes

**Version:** v0.2.0
**Release Date:** March 31, 2026

## Overview

The HyperFleet MVP Release (v0.2.0) delivers the Minimum Viable Product with enhanced observability, configuration management, and operational reliability. This release introduces OpenTelemetry tracing, CEL-based decision engines, improved connection pooling, and comprehensive operational documentation.

## Highlights

- **OpenTelemetry Tracing**: Complete distributed tracing for Sentinel with gRPC/HTTP exporters and CloudEvents integration
- **Viper Configuration**: Standardized configuration system across all components with improved validation
- **Connection Pooling**: pgbouncer sidecar support with enhanced reliability and timeout management
- **CEL Decision Engine**: Flexible, policy-driven decision making in Sentinel replacing hardcoded logic
- **Prometheus Integration**: ServiceMonitor resources for comprehensive metrics collection across all components

## Breaking Changes

### HyperFleet API

- **Configuration System Migration to Viper**
  - **Action Required**: Update configuration files and environment variables to new Viper-based schema
  - **Impact**: Legacy `_FILE` suffix environment variable support removed
  - **Reference**: [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)

### HyperFleet Sentinel

- **HyperShift HostedCluster Spec Migration**
  - **Action Required**: Verify cluster specifications compatibility with HyperShift format
  - **Impact**: May require updates to cluster definitions

## Known Issues

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

## Component Compatibility

All components at v0.2.0 are designed to work together. Deploy all components at the same version for optimal compatibility.

| Component | Version |
|-----------|---------|
| hyperfleet-api | v0.2.0 |
| hyperfleet-sentinel | v0.2.0 |
| hyperfleet-adapter | v0.2.0 |

## Getting Started

### Installation Guides

- **HyperFleet API**: [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
- **HyperFleet Sentinel**: [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
- **HyperFleet Adapter**: [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

### Container Images

```
quay.io/openshift-hyperfleet/hyperfleet-api:v0.2.0
quay.io/openshift-hyperfleet/hyperfleet-sentinel:v0.2.0
quay.io/openshift-hyperfleet/hyperfleet-adapter:v0.2.0
```

### Helm Installation

Helm charts are available in the respective component repositories. Clone the repositories and install locally:

```bash
# Clone component repositories
git clone https://github.com/openshift-hyperfleet/hyperfleet-api
git clone https://github.com/openshift-hyperfleet/hyperfleet-sentinel
git clone https://github.com/openshift-hyperfleet/hyperfleet-adapter

# Install from local charts
helm upgrade --install hyperfleet-api ./hyperfleet-api/charts/hyperfleet-api
helm upgrade --install hyperfleet-sentinel ./hyperfleet-sentinel/charts/hyperfleet-sentinel
helm upgrade --install hyperfleet-adapter ./hyperfleet-adapter/charts/hyperfleet-adapter
```

## Documentation

- **[Change Log](./CHANGELOG-v0.2.0.md)** - Detailed technical changes for all components
- **Component Guides** - [API](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md) | [Sentinel](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md) | [Adapter](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

## Security & Compliance

**Note**: This MVP release does not include formal security scanning or compliance validation. Security features and Konflux integration will be addressed in future releases. This release is intended for exploration and evaluation only, not for production use.

## Support

- **Issues**: Report in JIRA HYPERFLEET project or in the respective component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: See component operator guides above

---

**Important**: This MVP release is for exploration and evaluation only. Do not use in production environments.
