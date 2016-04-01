# Brooklyn Tendermint

Brooklyn blueprints for Tendermint application deployment.

This will contain the `.bom` files with Brooklyn catalog entries for the
Tendermint machines, and `.yaml` blueprints for deploying an application using
those machines.

## Tendermint Brooklyn Architecture

![Tendermint Brooklyn Architecture](docs/tendermint-brooklyn-architecture.png)

This diagram shows the components deployed by Brooklyn for a Tendermint MintNet
cluster, using the [`tendermint.bom`](tendermint-mintnet.bom) catalog entry.

## Getting Started

First, deploy a Clocker environment, using the latest release in the 1.1.x
stream. Once this is running, install the [tendermint-mintnet.bom](tendermint-mintnet.bom)
file in the Clocker catalog. Then, you must ensure you have access to an
archive containing the required data files for your mintnet application.

Files for the [Basecoin](https://github.com/tendermint/basecoin) application have been uploaded
as [basecoin.tgz](https://s3-eu-west-1.amazonaws.com/brooklyn-tendermint/basecoin.tgz)
for use in a blueprint, as follows:

```YAML
services:
- type: tendermint-mintnet
  brooklyn.config:
    tendermint.application.name: "basecoin"
    tendermint.application.archive: "https://s3-eu-west-1.amazonaws.com/brooklyn-tendermint/basecoin.tgz"
```

The full blueprint can be downloaded as [tendermint-basecoin.yaml](tendermint-basecoin.yaml).

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
