
在DNA区块链的节点模型中，节点分为**记账节点**和**同步节点**。

- 记账节点按照共识算法参与网络共识，产生区块。
- 同步节点只同步记账节点生成的区块。

## 1. 部署记账节点

默认情况下，客户端 `dnaNode CLI` 不会启动共识模块，需要通过 `--enable-consensus` 选项来开启共识。

```shell
dnaNode  --config=<config.json> --enable-consensus
```

默认情况下，客户端 `dnaNode CLI` 会启动 `RPC` 服务器，这可能会存在安全问题。因此，如果没有特殊要求，推荐使用 `--disable-rpc` 选项关闭 `RPC` 模块。

```shell
dnaNode  --config=<config.json> --disable-rpc
```

默认情况下，客户端 `dnaNode CLI` 会启动合约事件日志服务，这会降低节点的性能。因此，如果没有特殊需要，可以使用 `--disable-event-log` 选项关闭 `eventlog` 模块。

因此，我们推荐记账节点使用的启动参数如下：

```shell
dnaNode --config=<config.json> --enable-consensus --disable-rpc --disable-event-log
```

## 2. 部署同步节点

使用以下命令启动同步节点。

- 主网

  ```shell
  dnaNode --config=<主网配置文件>
  ```

- 测试模式

  测试模式用于搭建开发测试环境，是单节点网络。

  ```shell
  dnaNode --testmode
  ```

>在测试模式下，共识模块、 `RPC` 模块、 `Restful` 模块以及 `WebSocket` 模块会同时开启。

## 3. 常见问题

- 如何为节点指定创世区块？

  使用 `--config` 选项指定创世区块的配置文件。

- 如何为节点指定钱包文件？

  使用 `--wallet` 选项。

- 如何为节点指定账户？

  使用 `--account` 选项。
