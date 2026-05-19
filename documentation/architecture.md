# Network & Hardware Architecture

PLACEHOLDER TEMPLATE FOR IMPLEMENTATION LATER

This document tracks the physical hardware layout, network segmentation, and routing topology of the lab cluster.

---

## Physical Hardware Topology

| Node Name | Hostname | Hardware Specs | Role | IP Address |
| :--- | :--- | :--- | :--- | :--- |
| **Control Plane 1** | `k3shost` | [e.g., Mini PC / Custom Build Specs] | K3s Master / API Server | `192.168.1.X` |
| **Worker Node 1**   | `k3sworker1`| [e.g., Specs] | Workload Execution | `192.168.1.X` |

> **Future Hardware Roadmap:** Transition current compute hosts into a standardized, unified 10-inch server rack enclosure to optimize airflow, power distribution, and cable management.

---

## Network Segmentation (pfSense & VLANs)

The network is compartmentalized using a pfSense firewall to isolate cluster traffic from standard home traffic.

*   **LAN (`192.168.1.0/24`):** Trusted personal devices.
*   **MGMT / K3s Node Network (`192.168.X.0/24`):** Dedicated bare-metal OS layer for the K3s cluster nodes.
*   **Pod CIDR (`10.42.0.0/16`):** Internal Kubernetes cluster network (managed by K3s/Flannel).
*   **Service CIDR (`10.43.0.0/16`):** Internal Kubernetes service routing layer.

---

## Traffic Ingress & Edge Routing Flow

External traffic resolves securely without opening dangerous inbound ports on the local firewall:

1.  **Client Request:** A user hits a public domain name (e.g., `service.yourdomain.com`).
2.  **Cloudflare Edge:** DNS resolves to Cloudflare, which applies WAF protection, SSL/TLS termination, and access proxy rules.
3.  **Cloudflare Tunnel:** The request is pushed down an established, outbound-only connection directly into the `cloudflare-tunnel` pods running inside the K3s cluster.
4.  **Internal Routing:** The tunnel pod routes the traffic to internal cluster resources or forwards it to local reverse proxies (like Traefik) for advanced paths.
