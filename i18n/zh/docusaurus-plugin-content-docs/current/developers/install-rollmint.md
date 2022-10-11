---
sidebar_label: 安装Rollmint
---

# 设置Rollmint

在继续构建Wordle应用程序之前，我们需要设置 Rollmint在我们的代码库中。

## 安装Rollmint

在 `wordle` 目录中运行以下命令：

```sh
go mod编辑-替换github.com/cosmos/comosos sdk=github.com/cosmos-sdk-rollmint@v0.46.1-rollmint-v0.4.0
保持整洁
go mod下载
```

这样，我们将Rollmint更改添加到项目目录中。 现在，让我们构建 Wordle 应用！
