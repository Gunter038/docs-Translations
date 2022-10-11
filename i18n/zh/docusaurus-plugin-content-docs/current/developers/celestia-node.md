- - -
sidebar_label : 安装Celestia Node
- - -

# Celestia 节点

本教程介绍了构建和安装 celestia-node。 本教程假设您完成了[这里](./environment.md)的开发环境设置步骤。

## 安装 Celestia 节点

### Arabica 安装

安装 Arabia 开发网的 celestia-节点意味着安装一个与网络兼容特定版本。

通过运行以下命令安装 celestia-node 二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.1
make install
```

验证二进制文件是否正常工作并使用以下命令`celestia version`检查版本：

```console
$ celestia version
Semantic version: v0.3.1
Commit: 8bce8d023f9d0a1929e56885e439655717aea4e4
Build Date: Thu Sep 22 15:15:43 UTC 2022
System version: amd64/linux
Golang version: go1.19.1
```

### Mamaki 安装

安装 Mamaki 测试网的 celestia-节点意识着安装一个与网络兼容特定版本。

通过运行以下命令安装celestia-node二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

验证二进制文件是否正常工作并使用以下命令`celestia version`检查版本：

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```

## 网络选择

您可以在执行网络选择Celestia 节点在Arabica和Mamaki。 然而，您应该注意，网络在celestia节点上工作的最好 上述版本。

```sh
celestia light 初始
celestia light 开始--node.network arabica
```
