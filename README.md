# zabbix-stress-agents

Helm chart that deploys multiple Zabbix agent pods for stress testing a Zabbix server on OpenShift.

## How it works

The chart creates a StatefulSet with N replicas (default 10). Each agent pod uses `hostNetwork` to bind directly on the node IP, with a unique listen port (`portStart + ordinal`). Agents autoregister against the Zabbix server using hostnames `zabbix-agent-0`, `zabbix-agent-1`, etc.

A ServiceAccount and RBAC resources are created to grant the `hostnetwork-v2` SCC required by OpenShift.

## Install

```bash
helm install zabbix-stress ./zabbix-stress-agents
```

Override defaults as needed:

```bash
helm install zabbix-stress ./zabbix-stress-agents \
  --set agentCount=50 \
  --set zabbix.serverHost=192.168.1.10 \
  --set portStart=31000
```

## Configuration

| Parameter | Description | Default |
|---|---|---|
| `agentCount` | Number of agent pods to create | `10` |
| `image.repository` | Zabbix agent image | `registry.connect.redhat.com/zabbix/zabbix-agent-72` |
| `image.tag` | Image tag | `latest` |
| `zabbix.serverHost` | Zabbix server IP/hostname | `10.0.0.100` |
| `zabbix.metadata` | Autoregistration metadata string | `stress-test` |
| `portStart` | First listen port on the host (each agent increments by 1) | `30100` |
| `resources.requests.cpu` | CPU request per pod | `10m` |
| `resources.requests.memory` | Memory request per pod | `32Mi` |
| `resources.limits.cpu` | CPU limit per pod | `50m` |
| `resources.limits.memory` | Memory limit per pod | `64Mi` |

## Uninstall

```bash
helm uninstall zabbix-stress
```
