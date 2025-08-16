# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a Helm chart repository for Kubernetes applications maintained by Kubito. It contains production-ready charts for various applications including Cloudflared, Servarr stack, Excalidraw, SearXNG, and others.

## Architecture

- **Chart Structure**: Each chart follows standard Helm conventions with `Chart.yaml`, `values.yaml`, and `templates/` directory
- **Deployment Patterns**: Most charts include deployments, services, ingress, HPA, PVC, and service accounts
- **Multi-Component Charts**: Complex charts like `servarr` contain multiple sub-applications (Sonarr, Radarr, Jellyfin, etc.) organized in subdirectories
- **Resource Management**: Charts use consistent patterns for resource limits, persistent volumes, and horizontal pod autoscaling

## Key Commands

### Helm Operations
```bash
# Lint a chart
helm lint charts/<chart-name>

# Template rendering (dry-run)
helm template test charts/<chart-name> --dry-run

# Package a chart
helm package charts/<chart-name>

# Install from local chart
helm install <release-name> charts/<chart-name>

# Add the repository
helm repo add kubitodev https://charts.kubito.dev
```

### Development Workflow
- Charts are published to Artifact Hub at https://charts.kubito.dev
- Testing follows the strategy outlined in `TESTING.md` using Cypress and Goss
- Static analysis includes Trivy security scanning and `helm lint`

## Chart Patterns

### Standard Template Structure
- `deployment.yaml` - Main application deployment
- `service.yaml` - Kubernetes service
- `ingress.yaml` - Ingress configuration (when applicable)
- `hpa.yaml` - Horizontal Pod Autoscaler
- `pvc.yaml` - Persistent Volume Claims
- `serviceaccount.yaml` - Service account configuration
- `configmap.yaml` / `secret.yaml` - Configuration management

### Multi-Service Charts
The `servarr` chart demonstrates the pattern for charts with multiple related services:
- Each service has its own subdirectory under `templates/`
- Shared values and common patterns in root `values.yaml`
- Individual service configurations can be enabled/disabled

### Version Management
- Charts use semantic versioning
- `appVersion` tracks the upstream application version
- `version` tracks the chart version
- Both are maintained in `Chart.yaml`

## Testing Requirements

When modifying charts, ensure:
- Minimum 5 test cases per test type (functional/integration)
- Tests focus on deployment verification, not application functionality
- Tests must be stateless, independent, and retry-able
- Use `helm lint` for static analysis
- Follow the acceptance criteria in `TESTING.md`