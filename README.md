# HyperFleet Release Repository

Repository for hosting HyperFleet project release documentation.

## Overview

This repository contains release documentation for HyperFleet releases. For the MVP release, this includes release notes and changelog. Future production-ready releases will include comprehensive upgrade guides and security compliance documentation.

## Current Release: MVP (v0.2.0)

**Release Type**: Minimum Viable Product (MVP)

### 📄 Release Documents

- **[Release Notes](./RELEASE-NOTES-v0.2.0.md)** - What's new, breaking changes, getting started, and compatibility information
- **[Changelog](./CHANGELOG-v0.2.0.md)** - Detailed changes across all components

### 🚀 Components

The HyperFleet MVP Release includes three core components:

| Component | Repository |
|-----------|------------|
| hyperfleet-api | [openshift-hyperfleet/hyperfleet-api](https://github.com/openshift-hyperfleet/hyperfleet-api) |
| hyperfleet-sentinel | [openshift-hyperfleet/hyperfleet-sentinel](https://github.com/openshift-hyperfleet/hyperfleet-sentinel) |
| hyperfleet-adapter | [openshift-hyperfleet/hyperfleet-adapter](https://github.com/openshift-hyperfleet/hyperfleet-adapter) |

### 📚 Quick Start

1. Read the [Release Notes](./RELEASE-NOTES-v0.2.0.md) to understand what's included
2. Follow the component-specific operator guides:
   - [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
   - [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
   - [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

### ⚠️ Important Notes

- **Not Production Ready**: This MVP release is for exploration and evaluation only
- **Security**: Security scanning and compliance validation will be addressed in future releases with Konflux integration
- **Upgrade Path**: Formal upgrade procedures will be documented in future GA releases

## Support

- **Issues**: Report in JIRA HYPERFLEET project or in the respective component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: [Release Notes](./RELEASE-NOTES-v0.2.0.md) and component operator guides
