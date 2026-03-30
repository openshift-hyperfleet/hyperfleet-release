# HyperFleet MVP Release - Change Log

**Version:** v0.2.0
**Release Date:** March 30, 2026

## Overview

This changelog documents all changes included in the HyperFleet MVP Release (v0.2.0) across all components. Changes are organized by component and category.

---

## HyperFleet API v0.2.0

### Features

- Status Aggregation Logic
  - Enhanced aggregation logic for cluster and node pool status conditions
  - Improved status reporting accuracy

- Version Subcommand
  - Added CLI version subcommand for build information
  - Enables version verification and troubleshooting

- Connection Pooling with pgbouncer
  - pgbouncer sidecar support in Helm chart
  - Connection retry logic for sidecar startup races
  - Request-level context timeout in transaction middleware
  - Health check ping timeout configuration
  - Connection pool timeout configuration
  - Improved database connection reliability and performance

- Viper Configuration System
  - Complete refactor to Viper-based configuration
  - Streamlined config system aligned with architecture standards
  - Improved configuration validation and error reporting

- Condition Subfield Queries
  - Support for selective Sentinel polling via condition subfield filtering
  - More efficient polling with targeted queries

- PostgreSQL Advisory Locks
  - Migration coordination using advisory locks
  - Prevents race conditions during concurrent migrations
  - Improved deployment reliability

- Enhanced SliceFilter
  - Slice field validation
  - Star propagation support
  - More robust query filtering

### Bug Fixes

- Validate adapter status conditions in handler layer
  - Improved input validation and error handling

- Reject not operator for condition queries
  - Prevented unsupported query operations

- Align health ping timeout with K8s probe timeout
  - Fixed health check timing issues

- Truncate migrations table in CleanDB for test reliability
  - Improved test isolation and reliability

- pgbouncer secret handling and connection leak fixes
  - Fixed memory leaks and secret management issues

### Code Quality

- Address revive linter violations
  - Improved code quality and maintainability

- Expanded test coverage for errors package
  - Enhanced error handling reliability

- DatabaseConfig test coverage and advisory lock tests
  - Improved test coverage for database layer

### Documentation

- [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
  - Comprehensive operational documentation
  - Complete configuration examples

- Search and filtering documentation with detailed query syntax

- Connection pool and pgbouncer configuration guide

- Agent-native repo optimization with nested CLAUDE.md files
  - Improved development documentation structure

### Helm Chart

- Standardize appVersion and image.tag handling
  - Consistent version management across charts

- Align chart with Helm conventions standard
  - Improved chart quality and maintainability

- Remove org prefix from image.repository default
  - Simplified image configuration

- Use CHANGE_ME placeholder for image registry
  - Clearer configuration defaults

- Use 0.0.0-dev version for dev image builds
  - Better development workflow

### Breaking Changes

- Configuration system migrated to Viper
  - **Action Required**: Update configuration files and environment variables
  - Removed legacy `_FILE` suffix environment variable support

---

## HyperFleet Sentinel v0.2.0

### Features

- OpenTelemetry Tracing
  - Complete tracing implementation with gRPC/HTTP exporters
  - Support for all OTEL sampler types with configurable sampling
  - Trace context propagation to CloudEvents via traceparent extension
  - Span instrumentation for API calls, event publishing, and polling cycles
  - Protocol selection via OTEL_EXPORTER_OTLP_PROTOCOL environment variable
  - Enabled by default in v0.2.0
  - Comprehensive observability for distributed tracing

- CEL-based Decision Engine
  - Replaced hardcoded max_age with configurable CEL expression evaluation
  - Flexible, policy-driven decision making
  - Supports complex temporal and conditional logic

- Mock HyperFleet API
  - Development API for testing
  - Production-matching payload generation
  - Improved development and testing workflow

- Selective Query Filtering
  - Condition-based filtering for resource queries
  - More efficient API polling
  - Reduced network and processing overhead

- Poll Staleness Detection
  - Dead man's switch gauge metric
  - Configurable alert thresholds
  - Early detection of polling failures

- HyperShift HostedCluster Spec
  - Migrated from OCM spec to HyperShift HostedCluster specification
  - Improved cluster specification compatibility

### Bug Fixes

- Fix sentinel delays first event publish for newly created clusters
  - Improved event delivery timing
  - Reduced latency for new cluster detection

- Add fail fast check on HyperFleet API endpoint
  - Faster failure detection and recovery
  - Improved startup reliability

- Add structured logging to health check failure paths
  - Better troubleshooting and diagnostics

### Code Quality

- Align golangci-lint config with architecture standard
  - Improved code quality and consistency

### Documentation

- [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
  - Comprehensive operational documentation
  - Configuration examples and troubleshooting
- Reliability and observability metrics
- Alerting guidelines and SLI/SLO definitions

### Helm Chart

- Align chart with Helm conventions standard
  - Improved chart quality and maintainability

- Use image.tag for app.kubernetes.io/version label
  - Consistent version labeling

- Use 0.0.0-dev version for dev image builds
  - Better development workflow

### Cleanup

- Remove deprecated deployments/helm directory
  - Cleanup of deprecated code paths

### Breaking Changes

- HyperShift HostedCluster Spec Migration
  - **Action Required**: Verify cluster specifications are compatible with HyperShift format
  - May require updates to cluster definitions

---

## HyperFleet Adapter v0.2.0

### Features

- Time-Based Preconditions
  - Added `now()` custom CEL function for time-based stability preconditions
  - Enables waiting for resource stability based on age
  - Example: Wait 5 minutes after resource creation before taking action

- Prometheus Integration
  - Added ServiceMonitor resource for Prometheus discovery
  - Added Service resource for metrics collection
  - Complete metrics exposure for observability

- Configuration Standard
  - Implemented architecture-aligned configuration system
  - Viper support for flexible configuration management
  - Consistent with other HyperFleet components

### Bug Fixes

- Consolidate duplicate resource operation logs
  - Reduced log verbosity
  - Cleaner log output

- Fix time value inconsistency in documentation (>300s)
  - Corrected documentation errors

- Set failure status on nested discovery key collision
  - Improved error handling for configuration conflicts

- Preserve failed executionStatus on precondition error
  - Better error state management

### Code Quality

- Enable revive linter and resolve lint violations
  - Improved code quality

- Align golangci.yml with architecture standard v2 and resolve all lint violations
  - Comprehensive linter compliance
  - Enhanced code maintainability

### Documentation

- [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)
  - Time-based stability preconditions usage examples with CEL expressions
  - Required hyperfleet.io/generation annotation documentation

- Reliability documentation with runbook and metrics

### Helm Chart

- Standardize appVersion and image.tag handling
  - Consistent version management

- Align chart with Helm conventions standard
  - Improved chart quality

- Use 0.0.0-dev version for dev image builds
  - Better development workflow

- Skip version validation for 0.0.0-dev dev builds
  - Improved development experience

### Chores

- Remove unused GIT_TAG variable and Tag field
  - Code cleanup and simplification

---

## Cross-Component Changes

### Configuration Standardization

All components now use Viper-based configuration system aligned with HyperFleet architecture standards:
- Consistent configuration format across components
- Improved validation and error reporting
- Better development and operational experience

### Helm Chart Improvements

All component charts have been aligned with Helm conventions:
- Standardized version handling
- Consistent labeling
- Improved chart quality and maintainability
- Better development workflow with 0.0.0-dev versioning

### Observability Enhancements

- **Metrics**: All components expose Prometheus metrics
- **Tracing**: Sentinel includes comprehensive OpenTelemetry tracing
- **Logging**: Improved structured logging across all components
- **Monitoring**: ServiceMonitor resources for Prometheus integration

### Code Quality

All components have:
- Updated golangci-lint configurations
- Resolved linter violations
- Improved test coverage
- Enhanced code maintainability

### Documentation

Complete operational documentation suite:
- Operator guides for each component
- Reliability and observability documentation
- Configuration examples and references
- Troubleshooting procedures and runbooks

---

## Upgrade Considerations for MVP Release

### Required Actions

1. **Configuration Migration** (API and Adapter):
   - Update configuration to Viper format
   - Remove `_FILE` suffix environment variables (API only)
   - Review and test new configuration

2. **Cluster Spec Validation** (Sentinel):
   - Verify cluster specifications work with HyperShift format
   - Test cluster definitions before production deployment

3. **Database Migration** (API):
   - Backup database before upgrade
   - Verify PostgreSQL version compatibility (v13+)
   - Plan for potential downtime during migration

### Recommendations

1. **Test in Non-Production First**:
   - Deploy MVP Release (v0.2.0) to staging environment
   - Validate all functionality
   - Test upgrade path from v0.1.x to MVP

2. **Enable Observability**:
   - Configure Prometheus ServiceMonitors
   - Set up OpenTelemetry tracing (Sentinel)
   - Configure alerting for critical metrics

3. **Review Documentation**:
   - Read operator guides for each component
   - Review reliability documentation
   - Familiarize team with new features

---

## Known Issues

### Sentinel Memory/CPU Usage at Very Large Scale

- **Description**: Sentinel fetches the full resource set from the API and evaluates each resource in-memory via CEL. At very large scale, this may increase memory/CPU usage.
- **Severity**: LOW
- **Workaround**: Use `resource_selector` labels to shard across multiple Sentinel instances (horizontal scaling). See [multi-instance-deployment.md](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/multi-instance-deployment.md) for details.
- **Tracking**: [HYPERFLEET-805](https://redhat.atlassian.net/browse/HYPERFLEET-805)

---

## Contributors

Thank you to all contributors who made this release possible.

For detailed information about specific changes, please refer to the pull requests and issues linked in each entry.

---

**Document Version**: 1.0
**Last Updated**: March 30, 2026
