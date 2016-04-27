# Tendermint and Apache Brooklyn

Brooklyn blueprints for Tendermint application deployment using Clocker.

This contains the `.bom` files with Brooklyn catalog entries for the
Tendermint machines, and `.yaml` blueprints for deploying an application using
those machines.

## Tendermint Brooklyn Architecture

![Tendermint Brooklyn Architecture](docs/tendermint-brooklyn-architecture.png)

This diagram shows the components deployed by Brooklyn for a Tendermint MintNet
cluster, using the [`tendermint.bom`](tendermint-mintnet.bom) catalog entry.

Essentially there is a cluster of `tendermint-node` entities, which are just
containers for four `VanillaDockerApplication` entites running the *common*,
*data*, *code* and *app* Docker containers. Clocker ensures that all of the
containers for each node are deployed to the same Docker host using a
`GroupPlacementStrategy` which has been configured to require exclusive access
to the hosts for Tendermint.

Brooklyn can scale the cluster out by adding more of the nodes, which are all
configured identically. It also records information from the running containers
and this could be extended to interrogate the MintNet API for further data
to inform a scaling policy or simialr. This is not currently implemented
but there are many examples available if you want to experiment.

## Getting Started

First, deploy a Clocker environment, using the latest release in the 1.1.x
stream. Build Clocker from a checked out copy of its GitHub repository.

```Bash
% git clone git@github.com:brooklyncentral/clocker.git
...
% cd clocker
% mvn clean install
...
```

Once Clocker has started, you should start a Docker Cloud, preferably using
the Project Calico SDN, using your favourite cloud provider, or following
the instructions at the Brooklyn site on using Vargrant. The following
YAML snippet shows how to configre the `docker-cloud-calico` catalog
emtry, or just use the _Add Application_ wizard in the Clocker web UI.

```YAML
services:
- type: docker-cloud-calico
  brooklyn.config:
    entity.dynamicLocation.name: "calico-docker-cloud"
    docker.host.cluster.initial.size: 4
    docker.version: 1.10.3
    docker.host.securityGroup: "clocker"
```

You may also wish to install the Brooklyn CLI, which is packaged separately.

### Tendermint MintNet Blueprint

Once Docker is ready, deploy the [tendermint-mintnet.bom](tendermint-mintnet.bom)
file into the Clocker catalog. Then, you must ensure you have access to an
archive containing the required data files for your mintnet application, and
configure a blueprint to point to them.

### MintNet Basecoin Application Blueprint

Files for the [Basecoin](https://github.com/tendermint/basecoin) application
have been uploaded as [basecoin.tgz](https://s3-eu-west-1.amazonaws.com/brooklyn-tendermint/basecoin.tgz)
for use in a Brooklyn blueprint, as follows:

```YAML
location: calico-docker-cloud
services:
- type: tendermint-mintnet
  brooklyn.config:
    tendermint.node.count: 4
    tendermint.application.name: "basecoin"
    tendermint.application.archive: "https://s3-eu-west-1.amazonaws.com/brooklyn-tendermint/basecoin.tgz"
```

The full blueprint can be downloaded as [mintnet-basecoin.yaml](mintnet-basecoin.yaml).

Deploy this application to the Clocker cloud you created, usually named
`my-docker-cloud` or similar.

## Other Applications

There are four example mintnet application blueprints available:

1.  **basecoin** - [mintnet-basecoin.yaml](mintnet-basecoin.yaml)
2.  **counter** - [mintnet-counter.yaml](mintnet-counter.yaml)
3.  **localchain** - [mintnet-localchain.yaml](mintnet-localchain.yaml)
4.  **nomnomcoin** - [mintnet-nomnomcoin.yaml](mintnet-nomnomcoin.yaml)

# Further Reading

See the [Apache Brooklyn Website](https://brooklyn.apache.org/) for more
information and online documentation on YAML blueprints and configuring
Brooklyn. The [Clocker Wiki](https://github.com/brooklyncentral/clocker/wiki/)
has more documentation on Clocker internals, such as the placement strategies.
There are a number of [presentations](https://speakerdeck.com/grkvlt/) that
go into more detail about Clocker, and you can find the latest releases
via the [Clocker](http://clocker.io/) micro-site.

The [Tendermint Homepage](http://tendermint.com/) gives more information
about blockchain development, as does the [Consensus without
Mining](http://tendermint.com/docs/tendermint.pdf) paper. To find out more
about the blockchain itself, start with the
[Bitcoin](https://bitcoin.org/bitcoin.pdf) paper by Satoshi Nakamoto.

----

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
