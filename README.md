# Securing Multi-Tenant 5G MEC Nodes Using Preventative Policy-as-Code and Advanced Observability

This repository contains the complete infrastructure-as-code manifests and programmatic security configurations for simulating and hardening a resource-constrained **5G Multi-access Edge Computing (MEC)** node infrastructure. 

## Architectural Design Overview
The implementation utilizes a 3-node distributed topology managed via Kubernetes (`kind`), isolating core management, control plane, and dual worker runtime segments. The architecture is engineered to evaluate two core edge operational challenges:
1. **Telemetry Proportionality:** Minimizing the monitoring daemon footprint to prevent telemetry overhead from starving critical 5G user-plane processing.
2. **Preventative Multi-Tenancy Isolation:** Eradicating noisy-neighbor or resource-exhaustion attacks by intercepting deployment pipelines at the API Admission Controller layer.

## Repository Layout
```text
├── manifests/
│   ├── cnf-gateway.yaml       # Distributed 5G Data-Plane CNF Deployment
│   └── cnf-service.yaml       # ClusterIP Data Plane Routing Service
├── security/
│   ├── template-resource-limits.yaml  # OPA Gatekeeper Rego Engine Template
│   └── constraint-resource-limits.yaml  # Policy Enforcement Scope Rule
└── README.md                  # Evaluation Documentation
