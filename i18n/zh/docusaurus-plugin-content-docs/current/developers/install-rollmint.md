---
sidebar_label: Installing Rollmint
---

# Setting Up Rollmint

Before we continue with building our Wordle App, we need to set up Rollmint on our codebase.

## Installing Rollmint

在 `wordle` 目录中运行以下命令：

```sh
go mod edit -replace github.com/cosmos/cosmos-sdk=github.com/celestiaorg/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
go mod tidy
go mod download
```

With that, we have Rollmint changes added to the project directory. 现在，让我们构建 Wordle 应用！
