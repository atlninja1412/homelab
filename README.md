# Homelab

Self-hosted infrastructure running on Ubuntu 24.04 using Docker.

## Architecture

![Homelab Architecture](docs/architecture.png)

## Services

- Pi-hole (ad blocking)
- Nextcloud (cloud storage)
- Uptime Kuma (monitoring)
- Portainer (Docker UI)

## Networking

- Tailscale VPN for remote access
- No port forwarding required (T-Mobile gateway limitation)

## Goal

Fully reproducible, Git-managed homelab environment.
