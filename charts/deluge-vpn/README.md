# deluge Helm Chart with OpenVPN

This directory contains a Kubernetes chart to deploy a deluge server with optional OpenVPN support.

## Prerequisites Details

- Kubernetes 1.6+

## Chart Details

This chart will do the following:

- Implement a deluge deployment
- Optionally, deploy and connect to VPN provider using OpenVPN

Please note that only Mullvad VPN provider is implemented currently.

## Installing the Chart

To install the chart, use the following:

```console
$ helm repo add janvanbraeckel https://janvanbraeckel.com/helm-charts/
$ helm repo update

# Without VPN support
$ helm install --set vpn.enabled=false --name deluge janvanbraeckel/deluge

# With VPN support
$ VPN_USERNAME=myVpnUsername
$ VPN_PASSWORD=myVpnPassword
$ helm install --set vpn.enabled=true,vpn.username=$VPN_USERNAME,vpn.password=$VPN_PASSWORD --name deluge janvanbraeckel/deluge
```

## Configuration

The following table lists the configurable parameters for the deluge chart and their default values.

| Parameter                            | Description                              | Default                                                                         |
| ------------------------------------ | ---------------------------------------- | ------------------------------------------------------------------------------- |
| `image.registry`                     | Container registry                       | `docker.io`                                                                     |
| `image.repository`                   | Container image to use                   | `janvanbraeckel/delugevpn`                                                      |
| `image.tag`                          | Container image tag to deploy            | `v1.0.0`                                                                        |
| `image.pullPolicy`                   | Container pull policy                    | `IfNotPresent`                                                                  |
| `vpn.enabled`                        | Enable VPN support                       | `false`                                                                         |
| `vpn.protocol`                       | VPN protocol                             | `tcp`                                                                           |
| `vpn.username`                       | VPN username                             | `vpnUsername`                                                                   |
| `vpn.password`                       | VPN password                             | `vpnPassword`                                                                   |
| `vpn.provider`                       | VPN provider (Only mullvad is supported) | `mullvad`                                                                       |
| `vpn.hosts`                          | VPN host                                 | `be-bru-003.mullvad.net 80,be-bru-001.mullvad.net 80,be-bru-002.mullvad.net 80` |
| `vpn.port`                           | VPN port                                 | `1198`                                                                          |
| `vpn.lanNetwork`                     | Local host network                       | `10.0.0.0/8`                                                                    |
| `uiPort`                             | deluge UI port                           | `8080`                                                                          |
| `service.type`                       | Service type                             | `ClusterIP`                                                                     |
| `service.port`                       | Service port                             | `8080`                                                                          |
| `service.loadBalancerIP`             | Load balancer IP if `type=LoadBalancer`  | `""`                                                                            |
| `service.nodePort`                   | Node port if `type=NodePort`             | `""`                                                                            |
| `service.externalTrafficPolicy`      | External traffic policy                  | `Cluster`                                                                       |
| `service.annotations`                | Service annotations                      | `{}`                                                                            |
| `persistence.data.enabled`           | Data persistent for deluge settings      | `true`                                                                          |
| `persistence.data.accessMode`        | Data volume access mode                  | `ReadWriteOnce`                                                                 |
| `persistence.data.size`              | Data volume size                         | `128Mi`                                                                         |
| `persistence.data.storageClass`      | Data volume storage class                | `""`                                                                            |
| `persistence.downloads.enabled`      | Data persistent for deluge downloads     | `true`                                                                          |
| `persistence.downloads.accessMode`   | Data volume access mode                  | `ReadWriteOnce`                                                                 |
| `persistence.downloads.size`         | Data volume size                         | `10Gi`                                                                          |
| `persistence.downloads.storageClass` | Data volume storage class                | `""`                                                                            |
| `ingress.enabled`                    | Enable ingress generation                | `false`                                                                         |
| `ingress.annotations`                | Ingress annotations                      | `{}`                                                                            |
| `ingress.hosts[0].name`              | Host DNS name                            | `deluge.local`                                                                  |
| `ingress.hosts[0].path`              | Host path                                | `/`                                                                             |
| `ingress.hosts[0].tls`               | Enable TLS security                      | `true`                                                                          |
| `ingress.hosts[0].tlsHosts`          | Additional TSL host names                | `[]`                                                                            |
| `ingress.hosts[0].tlsSecret`         | TLS secret name                          | `deluge-tls-cert`                                                               |
| `extraEnvVars`                       | Additional env variables for deployment  | `{}`                                                                            |
| `strategy`                           | Pod upgrade strategy                     | `{}`                                                                            |
| `securityContext`                    | Pod security context                     | `{}`                                                                            |
| `resources`                          | Pod resource requests/limts              | `{}`                                                                            |
| `nodeSelector`                       | Pod node selector                        | `{}`                                                                            |
| `affinity`                           | Pod affinity                             | `{}`                                                                            |
