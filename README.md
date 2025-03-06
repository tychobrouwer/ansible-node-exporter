# Prometheus Node Exporter Ansible Role

This Ansible role installs and configures the [Prometheus Node Exporter](https://github.com/prometheus/node_exporter), a tool that exposes hardware and OS metrics for monitoring by Prometheus, on Debian-based systems. It automates the deployment of the Node Exporter binary, sets it up as a systemd service, and ensures it runs reliably to provide system metrics such as CPU, memory, disk, and network usage.

---

## Role Overview

The role performs the following tasks:
- Installs the Prometheus Node Exporter on a Debian-based system.
- Configures the Node Exporter to run as a systemd service.
- Allows customization of the installation directory and architecture.

This role is designed to be lightweight and straightforward, making it easy to integrate into a Prometheus monitoring setup for collecting system-level metrics from target hosts.

---

## Requirements

- **Operating System**: Debian-based system (e.g., Debian 11/12, Ubuntu 20.04/22.04).
- **Ansible**: Version 2.9 or higher.
- **Privileges**: Root or sudo access on the target host(s).
- **Dependencies**: 
  - `wget` or `curl` (for downloading the Node Exporter binary).
  - `tar` (for extracting the Node Exporter archive).
  - A working systemd installation (default on most modern Debian systems).

No additional Python modules or external roles are required beyond a standard Ansible setup.

---

## Role Variables

The role uses several variables to customize the installation and configuration of the Prometheus Node Exporter. Below are the available variables with their defaults (if applicable):

| Variable                | Description                                                                 | Default Value         |
|-------------------------|-----------------------------------------------------------------------------|-----------------------|
| `node_exporter_arch`    | Target architecture for Node Exporter (e.g., `amd64`, `arm64`).             | `amd64`              |
| `node_exporter_dir`     | Directory where Node Exporter will be installed.                            | `/opt`               |

---

## Example Playbook

Here’s an example Ansible playbook demonstrating how to use this role:

```yaml
---
- hosts: servers
  roles:
    - role: tychobrouwer.node_exporter  # Default installation

    - role: tychobrouwer.node_exporter
      node_exporter_arch: "amd64"
      node_exporter_dir: "/opt"
```

---

## Installation Steps (Performed by the Role)

1. **Download Node Exporter**: Fetches the specified version of Node Exporter for the target architecture from the official Prometheus GitHub releases.
2. **Extract and Install**: Extracts the Node Exporter binary to `node_exporter_dir` (e.g., `/opt/node_exporter`).
3. **Set Up Systemd Service**: Creates and enables a systemd service to manage Node Exporter, configured to listen on `node_exporter_port` with any specified `node_exporter_args`.
4. **Start Service**: Ensures Node Exporter is running and exposing metrics.

---

## Usage Notes

- **Metrics Access**: After installation, Node Exporter will expose metrics at `http://<server-ip>:9100/metrics`. Configure your Prometheus server to scrape this endpoint.
- **Custom Collectors**: Use `node_exporter_args` to enable or disable collectors based on your requirements. Refer to the [Node Exporter documentation](https://github.com/prometheus/node_exporter#collectors) for available options.
- **Verification**: Verify the service is running with `systemctl status node-exporter` and check metrics availability with `curl http://<server-ip>:9100/metrics`.

