# HyperFleet MVP Release - Change Log

**Version:** v0.2.0
**Release Date:** April 1, 2026

## Overview

This is the **first official release** of HyperFleet. This document details all features and capabilities delivered in the MVP across all components.

## Table of Contents

- [HyperFleet API v0.2.0 (MVP)](#hyperfleet-api-v020-mvp)
- [HyperFleet Sentinel v0.2.0 (MVP)](#hyperfleet-sentinel-v020-mvp)
- [HyperFleet Adapter v0.2.0 (MVP)](#hyperfleet-adapter-v020-mvp)

---

## HyperFleet API v0.2.0 (MVP)

- **API Endpoints and Resources**
  - **Clusters**: Create, Read, List operations
  - **NodePools**: Create, Read, List operations via cluster association (nested routes)
  - **Adapter Status**: Status reporting endpoints for both clusters and nodepools
  - Generation tracking for spec updates
  - Labels for categorization and filtering
  - Automatic metadata: created_time, updated_time, created_by, updated_by

- **Advanced Query and Filtering**
  - **Tree Search Language (TSL)** with operators: `=`, `!=`, `<`, `<=`, `>`, `>=`, `in`, `and`, `or`, `not`
  - Searchable fields: id, name, generation, created_by, updated_by, owner_id
  - **Label-based filtering**: `labels.<key>='value'` (e.g., `labels.environment='production'`)
  - **Status condition queries**: `status.conditions.<Type>='True'` (e.g., `status.conditions.Ready='True'`)
  - **Condition subfield queries**: last_updated_time, last_transition_time, observed_generation
  - Pagination with configurable page size (default: 100, max: 65500)
  - Sorting by any searchable field with ascending/descending order

- **Status Management**
  - Kubernetes-style status conditions: Available, Ready, and custom conditions
  - Condition fields: type, status, reason, message, observed_generation, timestamps
  - Adapter status upsert by adapter name (idempotent)
  - Automatic condition aggregation and synthesis
  - Support for arbitrary adapter metadata and data fields

- **Database Features**
  - PostgreSQL backend with GORM ORM
  - JSONB fields for flexible spec and condition storage
  - Soft delete pattern with deleted_at timestamps
  - Advisory lock-based migration coordination for rolling deployments
  - Connection pooling with PgBouncer sidecar support
  - Configurable request timeouts and connection pool limits

- **Observability and Monitoring**
  - Prometheus metrics endpoint on port 9090
  - Structured logging with OpenTelemetry integration
  - Request ID tracking for correlation
  - Data masking for sensitive fields

- **Health Checks**
  - Liveness probe `/healthz` - Always returns 200 OK when service is running
  - Readiness probe `/readyz` - Checks application ready state, graceful shutdown, and database connectivity
  - Database ping with configurable timeout (default: 2 seconds)
  - Returns 503 during startup, shutdown, or when database is unavailable
  - Separate health server on port 8080 with optional TLS support

- **Operational Features**
  - CLI subcommands: `serve`, `migrate`, `version`
  - OpenAPI Swagger UI at `/api/hyperfleet/v1/openapi.html`
  - RFC 9457 Problem Details error responses
  - Bearer token authentication (OAuth 2.0 compatible)
  - Development mode without authentication
  - TLS/HTTPS support
  - Provider-agnostic design via JSONB spec field

---

## HyperFleet Sentinel v0.2.0 (MVP)

- **Resource Monitoring and Polling**
  - Periodic resource polling at configurable intervals (default: 5 seconds)
  - Monitors both cluster and nodepool resources from HyperFleet API
  - Label-based resource filtering for horizontal sharding
  - Retry logic with exponential backoff (500ms-8s) for transient failures
  - Poll staleness detection with dead man's switch metric
  - Graceful degradation for invalid status during provisioning/deletion

- **CEL-Based Decision Engine**
  - Configurable decision logic using Common Expression Language (CEL)
  - Named parameter expressions with dependency resolution
  - Result expression determines event publishing
  - Access to resource object and `condition()` function for status queries
  - Timestamp comparisons and duration calculations
  - Topological sorting to detect circular dependencies
  - Default logic: checks for new resources, generation mismatches, staleness

- **CloudEvents Integration**
  - Publishes CloudEvents with type `com.redhat.hyperfleet.{resource_kind}.reconcile`
  - Unique event ID generation with UUID
  - Configurable CEL-based payload builder
  - Trace context propagation via traceparent extension
  - Variables available: resource, reason for dynamic payload generation

- **Message Broker Support**
  - Multi-broker abstraction: Google Cloud Pub/Sub, RabbitMQ, Stub/Mock
  - Configurable topic naming with environment variable overrides
  - Uses hyperfleet-broker library for publish operations
  - Pub/Sub emulator support for local development

- **Comprehensive Metrics**
  - 7 Sentinel-specific Prometheus metrics (plus broker metrics) with labels for resource_type and resource_selector
  - `hyperfleet_sentinel_pending_resources` - Resources pending reconciliation
  - `hyperfleet_sentinel_events_published_total` - Total events published
  - `hyperfleet_sentinel_resources_skipped_total` - Skipped resources with reason
  - `hyperfleet_sentinel_poll_duration_seconds` - Poll cycle latency histogram
  - `hyperfleet_sentinel_api_errors_total` - API errors by type
  - `hyperfleet_sentinel_broker_errors_total` - Broker errors by type
  - `hyperfleet_sentinel_last_successful_poll_timestamp_seconds` - Dead man's switch
  - Pre-built Grafana dashboard included

- **Horizontal Sharding**
  - Resource selector labels for multi-instance deployment
  - AND logic for multiple selectors (all must match)
  - Shard information included in metrics
  - Enables scaling by watching different resource subsets

- **Health Checks**
  - Liveness probe `/healthz` - Returns 503 if last poll exceeds 3x poll_interval
  - Readiness probe `/readyz` - Dependency checks (broker, sentinel poll) with graceful shutdown
  - Staleness detection for alerting on stuck services

---

## HyperFleet Adapter v0.2.0 (MVP)

- **Transporter Implementations**
  - **Kubernetes Transporter**: Direct Kubernetes API access with in-cluster or kubeconfig authentication
    - Operations: CreateResource, UpdateResource, ApplyManifest
    - Generation-aware apply operations (create/update/skip based on generation)
    - QPS and burst rate limiting
    - Resource discovery by name or label selectors
  - **Maestro Transporter**: Remote cluster management via Maestro/OCM ManifestWork
    - CloudEvents over gRPC communication with Maestro server
    - TLS/mTLS authentication support
    - ManifestWork operations: CreateManifestWork, GetManifestWork, ApplyManifestWork
    - Per-resource target cluster configuration
    - Server healthiness checks and keep-alive configuration

- **Precondition System**
  - Time-based preconditions with custom `now()` CEL function
  - API call preconditions with HTTP requests to external services
  - Field capture from API responses using JSONPath or CEL expressions
  - Condition evaluation with operators: equals, notEquals, in, notIn, contains, greaterThan, lessThan, exists
  - Full CEL expression support for complex precondition logic
  - Retry logic with exponential backoff for API calls

- **CEL Expression Engine**
  - Policy-driven decision making throughout adapter lifecycle
  - Custom CEL functions: `now()`, `toJson()`, `dig()`
  - Optional chaining for safe field access
  - Filter and size operations for collections
  - Full execution context: params, adapter metadata, resources, captured fields
  - Pre-compiled CEL expressions for performance

- **Resource Execution Pipeline**
  - Four-phase execution: Parameters → Preconditions → Resources → Post-Actions
  - Template rendering with Go templates
  - Discovery of existing resources by name or labels
  - Status reporting to external APIs via POST requests
  - Payload building with CEL expressions

- **Configuration and Deployment**
  - Viper-based configuration system with YAML and environment variable support
  - CloudEvents consumer for event-driven architecture
  - Configurable transporter selection per resource
  - Support for multiple adapter instances processing different event types

- **Observability and Monitoring**
  - Prometheus metrics endpoint on port 9090 with ServiceMonitor resource
  - Structured logging with execution context
  - Error tracking and reporting throughout pipeline phases

- **Health Checks**
  - Liveness probe `/healthz` - Always returns 200 OK when service is running
  - Readiness probe `/readyz` - Dependency checks (config loaded, broker subscribed) with graceful shutdown
  - Returns 503 when configuration loading fails or broker subscription fails
  - Immediate 503 response during graceful shutdown following HyperFleet standard
  - Optional `/config` debug endpoint for configuration troubleshooting

---

