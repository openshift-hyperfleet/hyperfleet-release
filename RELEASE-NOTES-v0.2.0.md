# HyperFleet MVP Release - Release Notes

**Version:** v0.2.0
**Release Date:** March 30, 2026

## Overview

The HyperFleet MVP Release (v0.2.0) is a significant milestone that delivers the Minimum Viable Product with enhanced observability, configuration management, and operational reliability across all components. This release introduces OpenTelemetry tracing, CEL-based decision engines, improved connection pooling, and comprehensive operational documentation.

## What's New

### Cross-Component Improvements

- **Configuration Standardization**: All components now use Viper-based configuration system aligned with architecture standards
- **Enhanced Observability**: Comprehensive metrics, tracing, and monitoring capabilities
- **Helm Chart Alignment**: All charts aligned with Helm conventions standard
- **Operational Documentation**: Complete operator guides and reliability documentation for each component

### HyperFleet API (v0.2.0)

#### Major Features

- **Status Aggregation Logic**: Enhanced aggregation logic for cluster and node pool status conditions
- **Version Subcommand**: Added CLI version subcommand for build information
- **Connection Pooling with pgbouncer**:
  - pgbouncer sidecar support in Helm chart
  - Connection retry logic for sidecar startup races
  - Request-level context timeout in transaction middleware
  - Health check ping timeout configuration
  - Connection pool timeout configuration
- **Viper Configuration System**: Complete refactor to Viper-based configuration with streamlined config system
- **Condition Subfield Queries**: Support for selective Sentinel polling via condition subfield filtering
- **PostgreSQL Advisory Locks**: Migration coordination using advisory locks to prevent race conditions
- **Enhanced SliceFilter**: Slice field validation and star propagation support

#### Key Improvements

- Validate adapter status conditions in handler layer
- Address revive linter violations
- Reject not operator for condition queries
- Align health ping timeout with K8s probe timeout
- Truncate migrations table in CleanDB for test reliability
- pgbouncer secret handling and connection leak fixes
- Expanded test coverage for errors package
- DatabaseConfig test coverage and advisory lock tests

### HyperFleet Sentinel (v0.2.0)

#### Major Features

- **OpenTelemetry Tracing**:
  - Complete tracing implementation with gRPC/HTTP exporters
  - Support for all OTEL sampler types with configurable sampling
  - Trace context propagation to CloudEvents via traceparent extension
  - Span instrumentation for API calls, event publishing, and polling cycles
  - Protocol selection via OTEL_EXPORTER_OTLP_PROTOCOL
  - Enabled by default with comprehensive documentation
- **CEL-based Decision Engine**: Replaced hardcoded max_age with configurable CEL expression evaluation
- **Mock HyperFleet API**: Development API for testing with production-matching payload generation
- **Selective Query Filtering**: Condition-based filtering for resource queries
- **Poll Staleness Detection**: Dead man's switch gauge metric with configurable alert thresholds
- **HyperShift HostedCluster Spec**: Migrated from OCM spec to HyperShift HostedCluster specification

#### Key Improvements

- Fix sentinel delays first event publish for newly created clusters
- Add fail fast check on HyperFleet API endpoint
- Add structured logging to health check failure paths
- Align golangci-lint config with architecture standard

### HyperFleet Adapter (v0.2.0)

#### Major Features

- **Time-Based Preconditions**: Added `now()` custom CEL function for time-based stability preconditions
- **Prometheus Integration**: Added ServiceMonitor and Service resources for Prometheus discovery and metrics collection
- **Configuration Standard**: Implemented architecture-aligned configuration system with Viper support

#### Key Improvements

- Consolidate duplicate resource operation logs
- Enable revive linter and resolve lint violations
- Align golangci.yml with architecture standard v2 and resolve all lint violations
- Set failure status on nested discovery key collision
- Preserve failed executionStatus on precondition error
- Document required hyperfleet.io/generation annotation

## Breaking Changes

### HyperFleet API

- **Configuration System Migration**: The configuration system has been migrated to Viper. Existing configuration files and environment variables must be updated to the new format.
  - **Action Required**: Review and update configuration files according to the new Viper-based schema
  - **Reference**: See API operator guide for complete configuration examples
- **Removed Legacy Environment Variable Support**: The `_FILE` suffix environment variable support has been removed
  - **Action Required**: Update deployment manifests to use the new configuration format

### HyperFleet Sentinel

- **HyperShift HostedCluster Spec Migration**: Migrated from OCM spec to HyperShift HostedCluster specification
  - **Action Required**: Verify compatibility with your cluster specifications
  - **Impact**: May require updates to cluster definitions

## Known Issues

### Sentinel Memory/CPU Usage at Very Large Scale

- **Description**: Sentinel fetches the full resource set from the API and evaluates each resource in-memory via CEL. At very large scale, this may increase memory/CPU usage.
- **Severity**: LOW
- **Workaround**: Use `resource_selector` labels to shard across multiple Sentinel instances (horizontal scaling). See the [Sentinel multi-instance deployment guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/multi-instance-deployment.md) for configuration details.
- **Tracking**: [HYPERFLEET-805](https://redhat.atlassian.net/browse/HYPERFLEET-805)

## Component Compatibility Matrix

| Component | Version | Compatible API Version | Compatible Sentinel Version | Compatible Adapter Version |
|-----------|---------|------------------------|----------------------------|---------------------------|
| hyperfleet-api | v0.2.0 | v0.2.0 | v0.2.0 | v0.2.0 |
| hyperfleet-sentinel | v0.2.0 | v0.2.0 | v0.2.0 | v0.2.0 |
| hyperfleet-adapter | v0.2.0 | v0.2.0 | v0.2.0 | v0.2.0 |

**Recommended Deployment**: All components should be deployed at the MVP version (v0.2.0) for optimal compatibility.

## Getting Started

This is an MVP release for exploration and evaluation. Please refer to the following guides for quick start and deployment instructions:

- **HyperFleet API**: [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
- **HyperFleet Sentinel**: [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
- **HyperFleet Adapter**: [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

**Note**: Formal upgrade guides will be provided in future production-ready releases.

## Container Images

Container images are available at:

```
# HyperFleet API
quay.io/openshift-hyperfleet/hyperfleet-api:v0.2.0

# HyperFleet Sentinel
quay.io/openshift-hyperfleet/hyperfleet-sentinel:v0.2.0

# HyperFleet Adapter
quay.io/openshift-hyperfleet/hyperfleet-adapter:v0.2.0
```

## Helm Charts

Helm charts for this release:

```bash
# Add HyperFleet Helm repository (if not already added)
helm repo add hyperfleet <helm-repository-url>
helm repo update

# Install or upgrade components
helm upgrade --install hyperfleet-api hyperfleet/hyperfleet-api
helm upgrade --install hyperfleet-sentinel hyperfleet/hyperfleet-sentinel
helm upgrade --install hyperfleet-adapter hyperfleet/hyperfleet-adapter
```

## Documentation

### Release Documentation
- [Change Log](./CHANGELOG-v0.2.0.md) - Detailed changes across all components

### Component Documentation
- [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md) - Deployment and operation guide for HyperFleet API
- [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md) - Deployment and operation guide for HyperFleet Sentinel
- [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md) - Guide for creating and deploying adapters

## Security & Compliance

**Note**: This MVP release does not include formal security scanning or compliance validation. Security features and Konflux integration for automated scanning will be addressed in future releases. This release is intended for exploration and evaluation only, not for production use.

## Getting Help

- **Issues**: Report issues in JIRA HYPERFLEET project or in the respective component repositories
- **Documentation**: Refer to operator guides in each component repository
- **Community**: #forum-hyperfleet

## Acknowledgments

This release includes contributions from the HyperFleet development team and community members. Thank you to all contributors who made this release possible.

---

**Important**: This MVP release is for exploration and evaluation only. Do not use in production environments.
