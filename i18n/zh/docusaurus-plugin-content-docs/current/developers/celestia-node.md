- - -
sidebar_label : 安装Celestia Node
- - -

# Celestia 节点

本教程介绍了构建和安装 celestia-node。 本教程假设您完成了[这里](./environment.md)的开发环境设置步骤。

## 安装 Celestia 节点

通过运行以下命令安装 celestia-node 二进制文件：

```sh
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
git checkout tags/v0.3.0-rc2
make install
```

验证二进制文件是否正常工作并使用以下`celestia version` 命令检查版本：

```console
$ celestia version
Semantic version: v0.3.0-rc2
Commit: 89892d8b96660e334741987d84546c36f0996fbe
```
