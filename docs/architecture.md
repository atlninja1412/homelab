# Homelab Architecture

## Overview

This homelab runs on a Surface Laptop 2 (Ubuntu 24.04) using Docker-based services and is remotely accessible via Tailscale.

---

## System Diagram

                         ┌──────────────────────────────┐
                         │        Internet / WAN         │
                         └──────────────┬───────────────┘
                                        │
                                        ▼
                 ┌──────────────────────────────────────┐
                 │  T-Mobile 5G Gateway (Router)        │
                 │  (No port forwarding control)         │
                 └──────────────┬───────────────────────┘
                                │ Ethernet
                                ▼
┌────────────────────────────────────────────────────────────┐
│        Surface Laptop 2 (Ubuntu 24.04 Server)              │
│                                                            │
│  ┌──────────────────────────────────────────────────────┐  │
│  │                NETWORK / ACCESS LAYER                │  │
│  │                                                      │  │
│  │  ┌──────────────┐        ┌──────────────────────┐   │  │
│  │  │  Tailscale   │◄──────►│  Remote Devices      │   │  │
│  │  │  (VPN Mesh)  │        │  Phone / Laptop      │   │  │
│  │  └──────────────┘        └──────────────────────┘   │  │
│  │                                                      │  │
│  │  Provides access outside NAT (no port forward needed)│  │
│  └──────────────────────────────────────────────────────┘  │
│                                                            │
│  ┌─────────────────────── CORE SERVICES ──────────────────┐ │
│  │                                                       │ │
│  │  ┌──────────────┐                                     │ │
│  │  │   Pi-hole    │  DNS Ad Blocking                   │ │
│  │  │ (Docker)     │◄──────────────┐                    │ │
│  │  └──────────────┘               │                    │ │
│  │                                  │ DNS               │ │
│  │  ┌──────────────┐               │                    │ │
│  │  │  Unbound     │◄──────────────┘                    │ │
│  │  │ (planned)    │  Recursive DNS                      │ │
│  │  └──────────────┘                                     │ │
│  │                                                       │ │
│  │  ┌──────────────┐                                     │ │
│  │  │ Nextcloud    │  Private Cloud Storage             │ │
│  │  │ (Docker)     │                                     │ │
│  │  └──────────────┘                                     │ │
│  │                                                       │ │
│  │  ┌──────────────┐                                     │ │
│  │  │ Uptime Kuma  │  Monitoring Dashboard              │ │
│  │  │ (Docker)     │                                     │ │
│  │  └──────────────┘                                     │ │
│  │                                                       │ │
│  │  ┌──────────────┐                                     │ │
│  │  │ Portainer    │  Docker Management UI              │ │
│  │  └──────────────┘                                     │ │
│  │                                                       │ │
│  └───────────────────────────────────────────────────────┘ │
│                                                            │
│  ┌──────────────────── INFRASTRUCTURE LAYER ──────────────┐ │
│  │                                                       │ │
│  │  Docker Engine                                        │ │
│  │  Docker Compose stacks                                │ │
│  │  Git-managed configs (homelab repo)                  │ │
│  └───────────────────────────────────────────────────────┘ │
│                                                            │
│  ┌──────────────────── STORAGE LAYER ─────────────────────┐ │
│  │                                                       │ │
│  │  2TB External Drive (⚠️ currently unstable)          │ │
│  │  (future: SSD recommended)                            │ │
│  └───────────────────────────────────────────────────────┘ │
└────────────────────────────────────────────────────────────┘

## Core Components

### Network Layer
- Tailscale VPN for secure remote access (no port forwarding required)
- T-Mobile 5G gateway (NAT, no inbound control)

### Services Layer
- Pi-hole → network-wide ad blocking
- Nextcloud → private cloud storage
- Uptime Kuma → service monitoring
- Portainer → Docker management UI

### Infrastructure Layer
- Docker Engine
- Docker Compose
- Git-based configuration management

### Storage Layer
- External 2TB HDD (temporary / unstable)
- Future upgrade: SSD recommended

---

## Design Goals

- Self-hosted privacy-first services
- Remote access without port forwarding
- Fully reproducible infrastructure via Git
- Modular Docker-based system
