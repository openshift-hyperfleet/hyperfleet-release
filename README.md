# HyperFleet Release Repository

This repository contains official release documentation for the HyperFleet project.

## First Official Release: v0.2.0 (MVP)

**Release Date**: April 1, 2026

This is the first official release of HyperFleet, delivering core cluster and node pool lifecycle management capabilities with event-driven architecture.

### Release Documentation

- **[Release Notes](./v0.2/RELEASE-NOTES.md)** - Overview, key features, and getting started
- **[Changelog](./v0.2/CHANGELOG.md)** - Technical details and component features

### Components

| Component | Version | Repository |
|-----------|---------|------------|
| hyperfleet-api | v0.2.0 | [openshift-hyperfleet/hyperfleet-api](https://github.com/openshift-hyperfleet/hyperfleet-api) |
| hyperfleet-sentinel | v0.2.0 | [openshift-hyperfleet/hyperfleet-sentinel](https://github.com/openshift-hyperfleet/hyperfleet-sentinel) |
| hyperfleet-adapter | v0.2.0 | [openshift-hyperfleet/hyperfleet-adapter](https://github.com/openshift-hyperfleet/hyperfleet-adapter) |

### Quick Start

1. Review the [Release Notes](./v0.2/RELEASE-NOTES.md)
2. Follow component-specific guides:
   - [API Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-api/blob/main/docs/api-operator-guide.md)
   - [Sentinel Operator Guide](https://github.com/openshift-hyperfleet/hyperfleet-sentinel/blob/main/docs/sentinel-operator-guide.md)
   - [Adapter Authoring Guide](https://github.com/openshift-hyperfleet/hyperfleet-adapter/blob/main/docs/adapter-authoring-guide.md)

### Important

**This MVP release is for exploration and evaluation only.**

- Not production-ready
- Security scanning and compliance validation planned for future releases
- No formal upgrade paths yet (first release)

### Support

- **Issues**: JIRA HYPERFLEET project or component repositories
- **Community**: #forum-hyperfleet
- **Documentation**: See release notes and operator guides above
