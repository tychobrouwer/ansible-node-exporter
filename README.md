Install and configure Prometheus Node Exporter
=========

This role installs and configures Prometheus.

Role Variables
--------------

Requirements
----------------

Example Playbook
----------------

```yaml
- hosts: servers
  vars:

  roles:
    - role: tychobrouwer.node_exporter

    - role: tychobrouwer.node_exporter
      node_exporter_arch: amd64
      node_exporter_dir: /opt
```

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
