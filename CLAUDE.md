# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Helm chart that deploys N Zabbix agent pods with individual NodePort services for stress testing a Zabbix server. Each agent gets a unique hostname (`zabbix-agent-XXXX`), listen port, and NodePort derived from `nodePortStart + index`.

## Common Commands

```bash
# Render templates locally (lint/validate)
helm template zabbix-stress .

# Install
helm install zabbix-stress .

# Install with overrides
helm install zabbix-stress . --set agentCount=50 --set zabbix.serverHost=192.168.1.10

# Upgrade after changes
helm upgrade zabbix-stress .

# Uninstall
helm uninstall zabbix-stress
```

## Architecture

- **`templates/agents.yaml`** — Single template that loops `agentCount` times, generating a Pod + NodePort Service pair per iteration. This is the only resource template.
- **`values.yaml`** — All configuration: agent count, image, Zabbix server connection, NodePort range start, resource limits.
- **`Chart.yaml`** — Chart metadata (appVersion tracks Zabbix version, currently 7.0).

Key design: each agent's `ZBX_LISTENPORT` equals its NodePort (`nodePortStart + i`), so the Zabbix server can reach agents directly via `<nodeIP>:<nodePort>`.
