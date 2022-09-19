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
git checkout tags/v0.3.0
make install
```

验证二进制文件是否正常工作并使用以下`celestia version` 命令检查版本：

```console
$ celestia version
Semantic version: v0.3.0
Commit: 00d80c423b2bfacec22a253ce6af3a534a1be3a7
```
