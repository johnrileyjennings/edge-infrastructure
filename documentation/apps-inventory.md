# Cluster Applications Inventory

A running inventory of all services currently deployed and managed via the GitOps pipeline inside the K3s cluster.

---

## Core Infrastructure Services

| Service Name | Namespace | Status | External Access | Storage Type |
| :--- | :--- | :--- | :--- | :--- |
| **Cloudflare Tunnel** | `cloudflare` | 🟢 Active | Outbound Edge Agent | Stateless |
| **HeadLamp** | `kubernetes-dashboard` | 🟢 Active | `lamp.eternallyzer0.com` | Stateless |
| **Vaultwarden** | `security` | ⏳ Planned | `vault.eternallyzer0.com` | PVC (Local Path) |

---

## Media & Storage Services (Planned Expansion)

This section maps out the incoming media array services targeted for K3s migration.

### Jellyfin (Media Server)
*   **Namespace:** `media`
*   **Deployment State:** ⏳ Planned
*   **Target Ingress:** Internal / Private LAN routing via Traefik.
*   **Storage Architecture:** Local high-capacity storage paths passed into pods via Persistent Volumes.

---

## Service Status Legend
*   🟢 **Active:** Fully deployed, verified operational, and synced via Flux.
*   🟡 **Degraded / Testing:** Manifests applied, but undergoing active configuration changes or troubleshooting.
*   ⏳ **Planned:** Defined in architectural roadmap; manifests pending construction.
