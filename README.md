# zabbix-stress-agents

Helm chart that deploys multiple Zabbix agent pods with individual NodePorts for stress testing a Zabbix server.

## How it works

The chart creates N agent pods (default 10), each with its own Service of type `NodePort`. Every agent gets a unique listen port and hostname (`zabbix-agent-0000`, `zabbix-agent-0001`, ...) and registers itself against the configured Zabbix server using the provided metadata string.

## Install

```bash
helm install zabbix-stress ./zabbix-stress-agents
```

Override defaults as needed:

```bash
helm install zabbix-stress ./zabbix-stress-agents \
  --set agentCount=50 \
  --set zabbix.serverHost=192.168.1.10 \
  --set nodePortStart=31000
```

## Configuration

| Parameter | Description | Default |
|---|---|---|
| `agentCount` | Number of agent pods to create | `10` |
| `image.repository` | Zabbix agent image | `zabbix/zabbix-agent` |
| `image.tag` | Image tag | `latest` |
| `zabbix.serverHost` | Zabbix server IP/hostname | `10.0.0.100` |
| `zabbix.metadata` | Autoregistration metadata string | `stress-test` |
| `nodePortStart` | First NodePort in the range (each agent increments by 1) | `30100` |
| `resources.requests.cpu` | CPU request per pod | `10m` |
| `resources.requests.memory` | Memory request per pod | `32Mi` |
| `resources.limits.cpu` | CPU limit per pod | `50m` |
| `resources.limits.memory` | Memory limit per pod | `64Mi` |

## Uninstall

```bash
helm uninstall zabbix-stress
```
