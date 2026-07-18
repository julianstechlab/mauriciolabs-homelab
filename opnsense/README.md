# OPNsense Firewall

## Overview

The OPNsense firewall acts as the central gateway for my homelab. It provides routing, firewall security, VLAN segmentation, DHCP, DNS resolution, and inter-VLAN communication while protecting the internal network from external threats.

---

## Table of Contents

- [Overview](#overview)
- [Hardware](#hardware)
- [Interface Architecture](#interface-architecture)
- [VLAN Configuration](#vlan-configuration)
- [Network Services](#network-services)
- [DHCP Configuration](#dhcp-configuration)
- [DNS Configuration](#dns-configuration)
- [Firewall Design](#firewall-design)
- [Security Principles](#security-principles)
- [Roadmap](#roadmap)

---
## Hardware

| Component | Details |
|-----------|---------|
| Device | Beelink EQ14 |
| CPU | Intel N150 |
| Memory | 16 GB DDR4 |
| Network Interfaces | 2 × 2.5 GbE Intel NICs |
| WAN | ISP Modem (DHCP) |
| LAN | Netgear GS308E Managed Switch |

---

## Interface Architecture

| Interface | Device | VLAN | IPv4 Address | Purpose |
|-----------|--------|------|--------------|---------|
| WAN | `re1` | N/A | DHCP | Connection to the ISP modem |
| LAN | `re0` | Native | 192.168.42.1/24 | Parent interface carrying all VLANs |
| Main (OPT1) | `vlan01` | 10 | 192.168.10.1/24 | Primary workstation network |
| Homelab (OPT2) | `vlan02` | 20 | 192.168.20.1/24 | Servers and infrastructure |
| AP (OPT3) | `vlan03` | 30 | 192.168.30.1/24 | Wireless and IoT devices |
| Media (OPT4) | `vlan04` | 40 | 192.168.40.1/24 | Media and gaming devices |


---

## VLAN Configuration

| VLAN | Interface | Network | Gateway | Purpose |
|------|-----------|---------|---------|---------|
| 10 | Main (OPT1) | `192.168.10.0/24` | `192.168.10.1` | Primary workstation network |
| 20 | Homelab (OPT2) | `192.168.20.0/24` | `192.168.20.1` | Servers and infrastructure |
| 30 | AP (OPT3) | `192.168.30.0/24` | `192.168.30.1` | Wireless and IoT devices |
| 40 | Media (OPT4) | `192.168.40.0/24` | `192.168.40.1` | Media and gaming devices |

---

## Network Services

| Service | Status | Purpose |
|---------|--------|---------|
| DHCP | ✅ Enabled | Assigns IP addresses on each VLAN |
| Unbound DNS | ✅ Enabled | Provides recursive DNS resolution and local hostname resolution |
| Dnsmasq | ❌ Disabled | Not currently in use |
| NAT | ✅ Enabled | Enables internet access for internal networks |
| VLAN Routing | ✅ Enabled | Routes permitted traffic between VLANs |
| Stateful Firewall | ✅ Enabled | Tracks connection states and filters network traffic |
| Local Hostname Resolution | ✅ Enabled | Resolves internal device hostnames |

---
## DHCP Configuration

Each VLAN uses its own DHCP scope and default gateway.

| VLAN | Gateway | DHCP Scope |
|------|---------|------------|
| 10 | `192.168.10.1` | *To be documented* |
| 20 | `192.168.20.1` | *To be documented* |
| 30 | `192.168.30.1` | *To be documented* |
| 40 | `192.168.40.1` | *To be documented* |

> **Note:** Exact DHCP ranges and static reservations will be added after they are verified in OPNsense.

---

## DNS Configuration

**Primary DNS Resolver:** Unbound DNS

### Current Features

- Recursive DNS resolution
- Local hostname resolution
- DNS caching
- DNS services available to each VLAN
- Internal name resolution across permitted networks

**Dnsmasq:** ❌ Disabled

---

## Firewall Design

The firewall follows the **Principle of Least Privilege**, allowing only the traffic required for each network to operate securely.

### Current Policy

- Allow outbound Internet access from internal VLANs
- Block unsolicited inbound traffic from the WAN
- Restrict unnecessary communication between VLANs
- Permit administrative access only from trusted devices
- Isolate wireless, IoT, media, and gaming devices from critical infrastructure
- Allow specific inter-VLAN traffic only when explicitly required

---

## Security Principles

### Network Segmentation

Devices are separated into dedicated VLANs based on their function and trust level.

### Principle of Least Privilege

Firewall rules allow only the minimum traffic necessary for each network.

### Stateful Inspection

OPNsense tracks connection states, allowing valid return traffic while blocking unauthorized connections.

### Controlled Inter-VLAN Routing

Traffic between VLANs is explicitly allowed or denied through firewall rules.

### Internal DNS Resolution

Unbound DNS provides local hostname resolution and caching without relying on individual client configurations.

---

## Roadmap

Planned improvements for the homelab:

- [ ] Create a dedicated Management VLAN
- [ ] Configure WireGuard VPN for secure remote access
- [ ] Deploy IDS/IPS
- [ ] Implement network-wide DNS filtering
- [ ] Configure centralized syslog logging
- [ ] Deploy Prometheus and Grafana for monitoring and visualization
