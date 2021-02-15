# C-Lightning

[C-Lightning](https://github.com/elementsproject/lightning) is an implementation of a
Lightning Network Node. The Lightning Network is a "Layer 2" payment protocol
that operates on top of a blockchain-based cryptocurrency. It features a
peer-to-peer system for making micropayments of cryptocurrency through a
network of bidirectional payment channels without delegating custody of funds.

## Introduction

This chart bootstraps a single C-Lightning node. The default docker image is taken from
[Kwinten De Backer](https://hub.docker.com/r/kiwiidb/c-lightning)'s dockerhub
repository. The docker image is the one from https://github.com/lightningd/plugins.It runs a mainnet node
with the [Sauron](https://github.com/lightningd/plugins/tree/master/sauron) plugin installed, which relies
on [our Blockstream overlords](https://blockstream.info) to fetch blocks (so no local bitcoind is required).
Of course this is antithetical to everything bitcoin is about but hey if I want to do this who is going to stop me.
I'll set up my own Esplora one day, I promise.

You can create a secret with an extra c-lightning config file. Eg. if you want to use a postgres database:

```
echo "wallet=postgres://YOUR_POSTGRES_CONNECTION_STRING" >> lightning.conf
kubectl create secret generic c-lightning-extra-config --from-file=./lightning.conf
```

This chart is based on [Fold's c-lightning chart](https://github.com/thesis/helm-charts)
## Prerequisites

* Kubernetes 1.8+
* PV provisioner support (for the `.lightning` directory)
* A Postgres database

## Installing the Chart

To install the chart with the release name `my-release`:

```
$ git clone https://github.com/kiwiidb/c-lightning-helm-chart
$ cd c-lightning-helm-chart
$ helm install --name my-release .
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
`extraConfig.enabled`   | If you want to mount a secret as extra c-lightning config file | `true`
`extraConfig.secret`   | Secret which contains extra c-ln config. Secret should have key `lightning.conf`| `c-lightning-extra-config`
`sauron.endpoint`   | Sauron API endpoint          | `https://blockstream.info/api`
`persistence.size`         | Size of persistent volume claim    | `5Gi`
`resources`                | CPU/Memory resource requests/limits| `{}`