# Edge-Infrastructure

This repository acts as the single source of truth for my multi-node K3s Kubernetes cluster, managed completely via **Flux v2** (GitOps) and secured using **SOPS + Age** encryption.

---

## Architecture Overview

*   **Orchestration:** K3s (Lightweight Kubernetes)
*   **GitOps Controller:** Flux v2 (auto-syncing this repo every 10 minutes)
*   **Secret Management:** Mozilla SOPS + Age (encrypted in Git, decrypted inside the cluster)
*   **Networking & Ingress:** Traefik / pfSense
*   **Edge Routing:** Cloudflare Tunnels (`cloudflared`)

### Repository Structure
```text
.
├── .gitignore
├── clusters/                # Flux system bootstrap targets
|   ├── host-device          # VM Running Ubuntu Server 24.04.4
|   └── RaspPi1-edge         # Raspberry Pi unit 1 edge device (Not emplemented)
├── documentaion             # Internal Documentation
|   ├── apps-inventory.md    # Active cluster services, URLs, and States
|   └── architecture.md      # Netowrk Topology, hardware specificatioins
└── k3s-manifests/           # Core cluster workloads
    ├── namespace.yaml       # Core namespace definitions
    ├── kustomization.yaml   # Root kustomize routing
    └── cloudflare-tunnel/   # Cloudflare edge connector stack
        ├── deployment.yaml
        ├── kustomization.yaml
        └── secret.enc.yaml  # SOPS encrypted tunnel token
