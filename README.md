
Here is the fully completed, cohesive README.md file. It merges your preferred high-level academic title and design overview with the updated folder path details for your files.

Run this single, self-terminating command block in your Ubuntu terminal to write the file cleanly without getting stuck:

Bash
cat << 'EOF' > ~/5g-mec-hardened-infrastructure/README.md
# Securing Multi-Tenant 5G MEC Nodes Using Preventative Policy-as-Code and Advanced Observability

This repository contains the complete infrastructure-as-code manifests and programmatic configurations for simulating and hardening a resource-constrained **5G Multi-access Edge Computing (MEC)** node infrastructure.

## Architectural Design Overview
The implementation utilizes a 3-node distributed topology managed via Kubernetes (`kind`), isolating core management, control plane, and dual worker runtime segments. The architecture is engineered to evaluate two core edge operational challenges:
1. **Telemetry Proportionality:** Minimizing the monitoring daemon footprint to prevent telemetry overhead from starving critical 5G user-plane processing.
2. **Preventative Multi-Tenancy Isolation:** Eradicating noisy-neighbor or resource-exhaustion attacks by intercepting deployment pipelines at the API Admission Controller layer.

## Repository Layout
```text
├── manifests/
│   ├── cnf-gateway.yaml          # Distributed 5G Data-Plane CNF Deployment
│   └── cnf-service.yaml          # ClusterIP Data Plane Routing Service
├── monitoring/
│   └── prometheus-values.yaml    # Low-Footprint Telemetry Helm Overrides
├── security/
│   ├── template-resource-limits.yaml  # OPA Gatekeeper Rego Engine Template
│   └── constraint-resource-limits.yaml  # Policy Enforcement Scope Rule
└── README.md                     # Evaluation Documentation
