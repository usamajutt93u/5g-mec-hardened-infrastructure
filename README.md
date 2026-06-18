# Securing Multi-Tenant 5G MEC Nodes Using Preventative Policy-as-Code and Advanced Observability

## Overview

This project demonstrates the deployment and hardening of a lightweight cloud-native 5G Multi-access Edge Computing (MEC) infrastructure using Kubernetes, Open Policy Agent (OPA) Gatekeeper, and Prometheus-based observability.

The implementation simulates a resource-constrained MEC node running containerized network functions (CNFs) while enforcing preventative security controls and low-overhead telemetry collection. The solution is designed for educational evaluation, DevSecOps experimentation, and telecom edge-computing research.

---

## Project Objectives

This implementation addresses four key objectives:

1. Deploy a hardened Cloud-Native Network Function (CNF) on Kubernetes.
2. Enforce preventative multi-tenant isolation using Policy-as-Code.
3. Provide lightweight observability suitable for resource-constrained MEC environments.
4. Demonstrate cloud-native DevSecOps practices aligned with telecom industry standards.

---

## Problem Statement

5G Multi-access Edge Computing environments introduce unique operational and security challenges:

- Multi-tenant workloads competing for limited resources
- Increased attack surface due to distributed edge locations
- Resource exhaustion and noisy-neighbor attacks
- Limited hardware capacity compared to centralized datacenters
- Need for continuous monitoring without introducing excessive overhead

Traditional security platforms often consume significant compute and memory resources, making them difficult to deploy directly on edge infrastructure.

This project evaluates how lightweight preventative controls and observability mechanisms can improve security while maintaining operational efficiency.

---

# Architecture Overview

```text
┌─────────────────────────────────────────────┐
│        Regional Core Datacenter             │
├─────────────────────────────────────────────┤
│ GitOps Platform (Argo CD)                   │
│ Jenkins CI/CD Pipeline                      │
│ Trivy Security Scanning                     │
│ Centralized Monitoring                      │
│ Enterprise Security Analytics               │
└─────────────────────────────────────────────┘
                     │
                     │
                     ▼
┌─────────────────────────────────────────────┐
│             MEC Edge Node (WSL)             │
├─────────────────────────────────────────────┤
│ Ubuntu 24.04                                │
│ Docker Engine                               │
│ Kind Kubernetes Cluster                     │
│                                             │
│ Control Plane Node                          │
│ Worker Node 1                               │
│ Worker Node 2                               │
│                                             │
│ OPA Gatekeeper                              │
│ Prometheus                                  │
│ CNF Gateway (2 Replicas)                    │
└─────────────────────────────────────────────┘
```

---

## MEC Security Strategy

The MEC node intentionally runs only lightweight security and observability services.

### Local Edge Controls

- OPA Gatekeeper
- Kubernetes Resource Constraints
- Non-Root Containers
- CPU and Memory Quotas
- Prometheus Monitoring

### Centralized Security Controls (Production Architecture)

The following services are architecturally positioned in regional datacenters rather than on edge nodes:

- Falco Runtime Threat Detection
- SPIFFE Workload Identity
- SIEM Correlation Platforms
- Enterprise Policy Management
- Advanced Threat Analytics

This approach minimizes edge resource consumption while preserving enterprise-grade security governance.

---

## Kubernetes Topology

The platform uses a distributed three-node Kubernetes cluster:

| Node | Role |
|--------|--------|
| Control Plane | Cluster Management |
| Worker 1 | CNF Runtime |
| Worker 2 | CNF Runtime |

The CNF deployment is replicated across worker nodes to improve availability and resiliency.

---

## CNF Hardening Features

The Nginx-based CNF gateway is deployed using security best practices.

### Security Controls

```yaml
runAsNonRoot: true
runAsUser: 101
allowPrivilegeEscalation: false
```

### Resource Controls

```yaml
requests:
  memory: "64Mi"
  cpu: "100m"

limits:
  memory: "128Mi"
  cpu: "200m"
```

### Availability Controls

- Two Replicas
- Kubernetes Self-Healing
- Cross-Node Scheduling

---

## Policy-as-Code Enforcement

OPA Gatekeeper provides preventative security by validating workload configurations before deployment.

### Enforced Policies

- Mandatory CPU Requests
- Mandatory Memory Requests
- Mandatory CPU Limits
- Mandatory Memory Limits

### Security Benefit

Workloads lacking resource governance are rejected at the Kubernetes Admission Controller layer before they can impact cluster stability.

---

## Observability Architecture

A lightweight monitoring stack is deployed to provide operational visibility with minimal resource overhead.

### Prometheus Metrics

- CPU Utilization
- Memory Utilization
- Pod Availability
- Deployment Health
- Service Throughput

### Load Testing

A BusyBox load-generator continuously generates requests against the CNF gateway to simulate MEC traffic conditions and validate telemetry collection.

---

## Repository Structure

```text
5g-mec-hardened-infrastructure/
│
├── manifests/
│   ├── cnf-gateway.yaml
│   └── cnf-service.yaml
│
├── monitoring/
│   └── prometheus-values.yaml
│
├── security/
│   ├── template-resource-limits.yaml
│   └── constraint-resource-limits.yaml
│
└── README.md
```

---

## Deployment Workflow

```text
Developer Commit
        │
        ▼
Kubernetes Manifest
        │
        ▼
OPA Gatekeeper Validation
        │
        ▼
Admission Controller
        │
        ▼
CNF Deployment
        │
        ▼
Prometheus Collection
        │
        ▼
Grafana Dashboard
```

---

## Validation Scenarios

### Scenario 1 – Hardened CNF Deployment

Objective:

Deploy a secure Cloud-Native Network Function with enforced resource constraints.

Validation:

```bash
kubectl get deployments
kubectl get pods -o wide
```

Expected Result:

- Deployment Available
- Pods Running
- Replicas Distributed Across Worker Nodes

---

### Scenario 2 – Policy Enforcement

Objective:

Prevent deployment of workloads that violate resource governance requirements.

Validation:

```bash
kubectl apply -f invalid-workload.yaml
```

Expected Result:

```text
admission webhook denied the request
```

---

### Scenario 3 – Telemetry Collection

Objective:

Generate synthetic traffic and observe workload metrics.

Validation:

```bash
kubectl logs load-generator
```

Expected Result:

- Increased Throughput
- Increased CPU Metrics
- Observable Prometheus Data
- Visible Grafana Metric Spikes

---

## Standards Alignment

| Standard | Alignment |
|-----------|------------|
| ETSI MEC | Multi-access Edge Computing Architecture |
| ETSI NFV | Cloud-Native Network Function Deployment |
| NIST SP 800-204C | Kubernetes Security Guidance |
| ITU-T Y.3100 | 5G Cloud-Native Architecture Principles |
| EU 5G Toolbox | Security Risk Mitigation Controls |
| UK NCSC Cloud Security Principles | Defense-in-Depth Approach |

---

## Academic Learning Outcomes

This project demonstrates practical application of:

- Cloud-Native Computing
- Kubernetes Administration
- DevSecOps Practices
- Infrastructure as Code
- Policy as Code
- Edge Computing Security
- Observability Engineering
- Telecom Infrastructure Hardening

---

## Future Enhancements

Potential production-scale improvements include:

- Argo CD GitOps Deployment
- Jenkins CI/CD Automation
- Trivy Vulnerability Scanning
- Falco Runtime Threat Detection
- SPIFFE Workload Identity
- Service Mesh Integration
- Centralized SIEM Correlation
- Multi-Cluster Federation

---

## Conclusion

The project demonstrates that lightweight preventative security controls and observability tooling can effectively secure and monitor a cloud-native 5G MEC platform without imposing excessive resource overhead.

By combining Kubernetes orchestration, OPA Gatekeeper policy enforcement, and Prometheus-based monitoring, the solution provides a practical balance between security, performance, and operational visibility suitable for resource-constrained edge environments.

---

## Author

**Usama Jutt**

MSc Cyber Security Candidate

Cloud-Native Telecom & DevSecOps Research Project
