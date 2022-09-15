---
sidebar_label: 安装 Optimint
---

# 设置 Optimint

在我们继续构建我们的 Wordle 应用程序之前，我们需要在我们的代码库上设置 Optimint。

## 安装 Optimint

在 `wordle` 目录中运行以下命令：

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk@v0.45.4-optimint-v0.3.5
go mod tidy
go mod download
```

这样，我们将 Optimint 更改添加到项目目录中。 现在，让我们构建 Wordle 应用！
