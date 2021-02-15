# C-Lightning

[C-Lightning](https://github.com/elementsproject/lightning) is an implementation of a
Lightning Network Node. The Lightning Network is a "Layer 2" payment protocol
that operates on top of a blockchain-based cryptocurrency. It features a
peer-to-peer system for making micropayments of cryptocurrency through a
network of bidirectional payment channels without delegating custody of funds.

## Introduction

This chart bootstraps a single C-Lightning node. The default docker image is taken from
[Kwinten De Backer](https://hub.docker.com/r/kiwiidb/c-lightning)'s dockerhub
repository. I intentionally made opionated choices: it runs a mainnet node, using a postgres database for channel management,
with the [Sauron](https://github.com/lightningd/plugins/tree/master/sauron) plugin installed, which relies
on [our Blockstream overlords](https://blockstream.info) to fetch blocks (so no local bitcoind is required).
Of course this is antithetical to everything bitcoin is about but hey if I want to do this who is going to stop me.
I'll set up my own Esplora one day, I promise.

This chart is based on [Fold's c-lightning chart](https://github.com/thesis/helm-charts)
## Prerequisites

* Kubernetes 1.8+
* PV provisioner support (for the `.lightning` directory)
* A Postgres database

## Installing the Chart

To install the chart with the release name `my-release`:

```
$ helm install --name my-release kiwiidb/c-lightning
```
## Configuration

The following tables list the configurable parameters of the c-lightning chart and
their default values.

Parameter                  | Description                        | Default
-----------------------    | ---------------------------------- | ----------------------------------------------------------
`image.repository`         | Image source repository name       | `kiwiidb/c-lightning`
`image.tag`                | `c-lightning` release tag.                 | `v0.9.3`
`image.pullPolicy`         | Image pull policy                  | `IfNotPresent`
`internalServices.rpcPort` | RPC Port                           | `9835`
`externalServices.p2pPort` | P2P Port                           | `9735`
`persistence.enabled`      | Save node state                    | `true`
`persistence.accessMode`   | ReadWriteOnce or ReadOnly          | `ReadWriteOnce`
`postgres.credentials.existingSecret`   | Secret with key connectionString and value the postgres connectionString| `c-lightning-postgres`
`sauron.endpoint`   | Sauron API endpoint          | `https://blockstream.info/api`
`extraArgs`   | C-Lightning extra args | `--log-level debug`
`persistence.size`         | Size of persistent volume claim    | `5Gi`
`resources`                | CPU/Memory resource requests/limits| `{}`