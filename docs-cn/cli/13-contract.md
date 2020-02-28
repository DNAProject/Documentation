
DNA区块链客户端 `DNA-CLI` 提供了智能合约模块，可以在命令行中通过 `contract` 命令实现如下功能：

- `NeoVm` 智能合约的部署。
- `NeoVm` 智能合约的执行。
- `NeoVm` 智能合约的预执行。

我们以 `hello_dna.py` 智能合约为例，在本地测试网上进行命令的讲解。

```python
from DNA.interop.System.Runtime import Notify


def main(operation, args):
    if operation == 'echo':
        return echo(args[0])
    return False

def echo(msg):
    Notify(['echo', msg])
    return msg

```

将该段智能合约代码使用编译器编译后可以得到相应的 `AVM` 字节码，将其以 `hello_dna.avm` 命名保存到本地。

```shell
51c56b6c58c56b6a00527ac46a51527ac46a52527ac46a51c3046563686f7d9c7c75642700006a53527ac46a52c300c3516a53c3936a53527ac46a53c36a00c365f2006c7566620300006c75660111c56b6a00527ac46a51527ac46a51c300947600a0640c00c16a52527ac4620e007562030000c56a52527ac46a52c3c0517d9c7c75641c00006a53527ac46a52c300c36a54527ac4516a55527ac4625c006a52c3c0527d9c7c756421006a52c300c36a53527ac46a52c351c36a54527ac4516a55527ac4616232006a52c3c0537d9c7c756424006a52c300c36a53527ac46a52c351c36a54527ac46a52c352c36a55527ac462050000f100c176c96a56527ac46a53c36a57527ac46a57c36a54c37d9f7c756419006a56c36a57c3c86a57c36a55c3936a57527ac462e0ff6a56c36c756656c56b6a00527ac46a51527ac46a52527ac4620300046563686f6a52c352c176c9681553797374656d2e52756e74696d652e4e6f746966796a52c36c7566
```

其中，你可以用来编译智能合约的工具如下：

- [SmartX](https://smartx.dnaproject.org/)
- [DNA-python-compiler](https://github.com/dnaproject/DNA-python-compiler)

此外，你需要开启一个终端，启动本地测试网。

```shell
dnaNode --testmode
```

## 1. 部署智能合约

在智能合约模块中， `deploy` 命令用于部署智能合约，其参数如下：

- `--code`：指定存储智能合约 `AVM` 字节码文件的路径。

- `--name`：指定智能合约的名称。

- `--version`：指定智能合约的版本号。

- `--author`：指定智能合约的作者信息。

- `--email`：指定智能合约的联系人电子邮件。

- `--desc`：指定智能合约的描述信息。

- `--prepare, -p`：指定智能合约的预部署。

```shell
$ dnaNode contract deploy --code  .\hello_dna.avm --name hello_dna --version 1.0.0 --author NashMiao --email contact@example.com --desc hello_dna -p
Contract pre-deploy successfully.
```

```shell
$ dnaNode contract deploy --code  .\hello_dna.avm --name hello_dna --version 1.0.0 --author NashMiao --email contact@example.com --desc hello_dna
Password:
Deploy contract:
  Contract Address:0203e74032b6b65de9872180f9b600f13858357d
  TxHash:ffb0a02847d31641f05a498a19a4f9f7e7d7616d0ab163c641d16d11fb02955e

Tip:
  Using './dnaNode info status ffb0a02847d31641f05a498a19a4f9f7e7d7616d0ab163c641d16d11fb02955e' to query transaction status.
```

在输出的信息中，`Contract Address` 为合约的哈希地址，是对智能合约接口进行调用的依据。

根据返回的交易哈希，我们可以查询智能合约部署交易的执行状态， `State` 字段为 `1`，表示智能合约部署成功。

```shell
$ dnaNode info status ffb0a02847d31641f05a498a19a4f9f7e7d7616d0ab163c641d16d11fb02955e
Transaction states:
{
   "TxHash": "ffb0a02847d31641f05a498a19a4f9f7e7d7616d0ab163c641d16d11fb02955e",
   "State": 1,
   "GasConsumed": 0,
   "Notify": []
}
```

>
通过 `dnaNode info status <TxHash>` 命令查询合约执行状态，如果返回 `UNKNOWN TRANSACTION`，表示该交易尚未被打包到区块中，可能有多种情况：
* 交易还在交易池中排队等待被打包。
* 交易因为 `gaslimit` 设置过低，导致交易被拒绝。


## 2. 调用智能合约

DNA区块链客户端 `dnaNode-CLI` 支持以下参数类型：

- `array`：对象数组，数组元素可以是 `NeoVM` 支持的任意数量、任意类型的值。
- `bytearray`：字节数组，输入时需要将字节数组编码成十六进制字符串，如 `[]byte("HelloWorld")` 编码成十六进制字符串 `48656c6c6f576f726c64`。
- `string`：字符串字面值。
- `int`：整数。
- `bool`：布尔型变量，用 `true` 和 `false`表示。

> **`NeoVM` 虚拟机不支持浮点数值，需要将浮点数转换成整数。**

`dnaNode-CLI` 使用前缀法构造参数，参数使用类型标识标注类型，多个参数使用 `,` 分隔。

- 字符串参数表示为 `string:hello`。
- 整数参数表示为 `int:10`。
- 布尔类型参数表示为 `bool:true`。
- 对象数组 `array` 类型用 `[]` 表示数组元素范围，如 `[int:10, string:hello, bool:true]`。

由于在DNA区块链网络中，智能合约的调用分为执行和预执行。因此，我们也将分别介绍两种合约的调用方式。

### 2.1 智能合约的执行

```shell
$ dnaNode contract invoke --address 0203e74032b6b65de9872180f9b600f13858357d --params string:echo,[string:dna]
Invoke:7d355838f100b6f9802187e95db6b63240e70302 Params:["echo",["dna"]]
Password:
  TxHash:8d62fb42647c175dd5689dd5d1062dda093d59c66ca22f710a2b47174c7ed966

Tips:
  Using './dnaNode info status 8d62fb42647c175dd5689dd5d1062dda093d59c66ca22f710a2b47174c7ed966' to query transaction status.
```

当我们执行智能合约时，将会得到对应的交易哈希 `TxHash`。

通过交易哈希，我们能够查询到该笔交易当前的状态、所在的区块（若执行成功）、触发的事件（若执行成功）等信息。因此，交易哈希是我们与区块链世界沟通的桥梁。

```shell
$ dnaNode info status 8d62fb42647c175dd5689dd5d1062dda093d59c66ca22f710a2b47174c7ed966
Transaction states:
{
   "TxHash": "8d62fb42647c175dd5689dd5d1062dda093d59c66ca22f710a2b47174c7ed966",
   "State": 1,
   "GasConsumed": 0,
   "Notify": [
      {
         "ContractAddress": "0203e74032b6b65de9872180f9b600f13858357d",
         "States": [
            "6563686f",
            "646e61"
         ]
      }
   ]
}
```

通过查询交易哈希，我们获得了如下信息。

- 交易已经执行成功：`State` 字段为 `1`。
- 该笔交易未消耗 `gas`：`GasConsumed` 字段为 `0`。
- 该笔交易在地址为 `0203e74032b6b65de9872180f9b600f13858357d` 的合约中触发了 `echo` 事件，事件的值为 `dna`。

其中，利用交易哈希所查询到的序列化后的十六进制字符串，可以利用  `DNA-python-sdk` 快速进行反序列化。

```python
from DNA.utils.contract_data import ContractDataParser


event_name = ContractDataParser.to_utf8_str('6563686f')
event_value = ContractDataParser.to_utf8_str('646e61')
```

```shell
$ dnaNode contract invoke --address 0203e74032b6b65de9872180f9b600f13858357d --params string:echo,[string:dna] 
Invoke:7d355838f100b6f9802187e95db6b63240e70302 Params:["echo",["dna"]]
Password:
  TxHash:c6d34bf17eaf467bc47748408e6704d5fcd054269a0d0ca985c387eb930d79b8

Tips:
  Using './dnaNode info status c6d34bf17eaf467bc47748408e6704d5fcd054269a0d0ca985c387eb930d79b8' to query transaction status.
```

```shell
$ dnaNode info tx c6d34bf17eaf467bc47748408e6704d5fcd054269a0d0ca985c387eb930d79b8
{
   "Version": 0,
   "Nonce": 1548686936,
   "GasPrice": 0,
   "GasLimit": 200000,
   "Payer": "AL4m8xiSrmxAjCEGVdZWADQgVuasozpnF9",
   "TxType": 209,
   "Payload": {
      "Code": "086f6e746f6c6f677951c1046563686f677d355838f100b6f9802187e95db6b63240e70302"
   },
   "Attributes": [],
   "Sigs": [
      {
         "PubKeys": [
            "02b76e86481df6e9814b203a41a0b040c87807d1bcfd18f8ea69c7d73ebf2b944a"
         ],
         "M": 1,
         "SigData": [
            "f7ca63555b6bc9d9f5d82cccb2a6561d5a1a68211d25b211f5597c98ca10cadbe63d399e767c4963195ee7e8154b556f0cc80da950809c86f48f67ecd2488df8"
         ]
      }
   ],
   "Hash": "c6d34bf17eaf467bc47748408e6704d5fcd054269a0d0ca985c387eb930d79b8",
   "Height": 3017
}
```

### 2.2 智能合约的预执行

我们可以使用 `-p` 参数预执行智能合约，获得执行结果以及 `GAS` 消耗。

```shell
$ dnaNode contract invoke --address 0203e74032b6b65de9872180f9b600f13858357d --params string:echo,[string:dna] -p
Invoke:7d355838f100b6f9802187e95db6b63240e70302 Params:["echo",["dna"]]
Contract invoke successfully
  Gas limit:20000
  Return:01 (raw value)
```

同时，我也可以使用 `--return` 选项指明返回值的类型，获得解析后的返回值。

```shell
$ dnaNode contract invoke --address 0203e74032b6b65de9872180f9b600f13858357d --params string:echo,[string:dna] -p --return bool
Invoke:7d355838f100b6f9802187e95db6b63240e70302 Params:["echo",["dna"]]
Contract invoke successfully
  Gas limit:20000
  Return:true
```

>智能合约在执行之前，可以通过预执行，试算出当前执行所需要的 `gas limit`，避免`gas limit`不足导致执行失败。
