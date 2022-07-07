# Celestia Node

This tutorial goes over installing and building Celestia Node. This tutorial presumes you completed the steps in setting up your own environment [here](./environment.md).

## Install Celestia Node

Install the Celestia Node binary by running the following command:

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

Verify that the binary is working and check the version with `celestia version` command:

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```
