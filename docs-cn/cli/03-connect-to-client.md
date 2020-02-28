
DNA区块链客户端提供了大量的调用接口，你可以通过 `RPC` 、 `Restful` 或者 `WebSocket` 进行访问。

默认情况下，`Restful` 接口监听在 `20334` 端口，`Websocket` 接口监听在 `20335` 端口，`RPC` 接口监听在 `20336` 端口。

## 1. 基于客户端进行调用

DNA区块链客户端提供了大量调用命令，这些命令默认会向本地启动的节点按照 `JSON-RPC` 协议发送调用命令。

一个简单的例子是分别在测试模式和主网同步模式查询账户余额。

1. 在第一个终端中开启测试模式：

   ```shell
   ./dnaNode --testmode
   ```

2. 在第二个终端中查询区块信息

   ```shell
   ./dnaNode info block <block-no>
   ```

3. 在第二个终端中查询交易信息

   ```shell
   ./dnaNode info tx <tx-hash>
   ./dnaNode info status <tx-hash>
   ```

## 2. 基于 SDK 进行调用

DNA区块链提供了众多 `SDK` 供开发者使用，你可以参考下表选择自己熟悉的语言，更多关于 `SDK` 的信息你可以点击[ 这里 ](https://dev-docs.dnaproject.org/#/docs-cn/SDKs/00-overview)访问文档中心的 `SDK` 部分获取。

|          库          |    语言     |                        项目地址                         |
| :------------------: | :---------: | :-----------------------------------------------------: |
|   DNA-ts-sdk    | TypeScript  |        https://github.com/DNAProject/DNA-ts-sdk         |
|   DNA-go-sdk    |     Go      |        https://github.com/DNAProject/DNA-go-sdk         |
|  DNA-java-sdk   |    Java     |       https://github.com/DNAProject/DNA-java-sdk        |
| DNA-python-sdk  |   Python    |      https://github.com/DNAProject/DNA-python-sdk       |

以 `DNA-python-sdk` 为例，你可以在安装 `SDK` 后用简洁的代码快速连接到运行中的节点。

- 安装 SDK

```shell
pip install DNA-python-sdk
```

- 连接到主网

```python
from DNA.sdk import DNA


sdk = DNA()
sdk.rpc.connect_to_main_net()
sdk.restful.connect_to_main_net()
sdk.websocket.connect_to_main_net()
```


- 连接到本地节点

```python
from DNA.sdk import DNA


sdk = DNA()
sdk.rpc.connect_to_localhost()
sdk.restful.connect_to_localhost()
sdk.websocket.connect_to_localhost()
```

- 连接到自定义节点

```python
from DNA.sdk import DNA


sdk = DNA()
rpc_address = 'http://localhost:20336'
restful_address = 'http://localhost:20334'
websocket_address = 'http://localhost:20335'
sdk.rpc.set_address(rpc_address)
sdk.restful.set_address(restful_address)
sdk.websocket.set_address(websocket_address)
```

<p class="info">一般情况下，你只需要连接到 `RPC` 、 `Restful`  或 `WebSocket` 中的一个接口，并使用该接口与节点进行交互即可。</p>

关于函数库的更多信息可以在下列章节中找到：

- [DNA-ts-sdk](docs-cn/SDKs/ts-sdk.md)
- [DNA-java-sdk](docs-cn/SDKs/java-sdk.md)
- [DNA-python-sdk](docs-cn/SDKs/python-sdk.md)

## 3. 使用公开节点

通常情况下，开发者自己不方便于自己搭建区块链网络。
因此，DNA区块链提供了测试网节点以及主网节点供开发者使用，它们均支持 `RPC` 、 `Restful` 以及 `WebSockek` 调用，并使用默认的端口号。

- 测试网节点
  - [http://polaris1.dnaproject.org](http://polaris1.dnaproject.org)
  - [http://polaris2.dnaproject.org](http://polaris2.dnaproject.org)
  - [http://polaris3.dnaproject.org](http://polaris3.dnaproject.org)
  - [http://polaris4.dnaproject.org](http://polaris4.dnaproject.org)
  - [http://polaris5.dnaproject.org](http://polaris5.dnaproject.org)

- 主网节点
  - [http://dappnode1.dnaproject.org](http://dappnode1.dnaproject.org)
  - [http://dappnode2.dnaproject.org](http://dappnode2.dnaproject.org)
  - [http://dappnode3.dnaproject.org](http://dappnode3.dnaproject.org)
  - [http://dappnode4.dnaproject.org](http://dappnode4.dnaproject.org)

