

* Native 合约 ```API```： 有关DNA网络中使用的 ```Native``` 合约 ```API``` ，请参考 [Native Contract API](https://github.com/dnaproject/dna/blob/master/docs/specifications/native_contract/paramapi.md)。
* Python 合约 `API`： 有关DNA网络中使用的 ```Python``` 合约 ```API``` 和示例，请参考 [API doc](https://apidoc.dnaproject.org/smartcontract/)。


为了方便开发者快速开始智能合约编写，我们在```SmartX```上提供了众多实用的合约模板。

### 基础合约模板

Token 转账和存储是合约里较基础的功能。

#### 合约里支持 Token 转账

```python
Cversion = '2.0.0'
from DNA.interop.DNA.Native import Invoke
from DNA.builtins import state
from DNA.interop.System.Runtime import Notify
from DNA.interop.System.ExecutionEngine import GetExecutingScriptHash
from DNA.interop.DNA.Runtime import Base58ToAddress,AddressToBase58


# Token Big endian Script Hash: 0x0200000000000000000000000000000000000000
TokenContract = Base58ToAddress("AFmseVrdL9f9oyCzZefL9tG6UbvhfRZMHJ")


def Main(operation, args):
    if operation == "transferToken":
        if len(args) != 3:
            return False
        return transferToken(args[0], args[1], args[2])

    return False

def transferToken(from_base58, to_base58,  token_amount):
    from_acct = Base58ToAddress(from_base58)
    to_acct = Base58ToAddress(to_base58)
    param = state(from_acct, to_acct, token_amount)
    res = Invoke(0, TokenContract, "transfer", [param])
    if res and res == b'\x01':
        Notify([True,from_base58, to_base58,  token_amount])
        return True
    else:
        Notify([False,from_base58, to_base58,  token_amount])
        return False
```

#### 链上数据的存储

某些特定类型的 ```dApp``` 可能有往链上存取数据的需求，本文档介绍了如何在链上进行数据存取操作。

链上数据的存储主要在于通过智能合约进行操作，以下是一个智能合约的例子,请在 ```SmartX``` 中调试，请参考 [调试指南](../QuickGuide/00-dapp_development)

```python
Cversion = '2.0.0'
"""
Contract Record
"""
from DNA.interop.System.Storage import GetContext, Get, Put
from DNA.interop.System.Runtime import CheckWitness, Notify
from DNA.interop.DNA.Runtime import Base58ToAddress

# set the owner codeAddr to your base58 codeAddr (starting with a captical A)
Owner = Base58ToAddress('APa7uMYqdqpFK2chwwmeE7SrQAWZukuGbX')

def Main(operation, args):
    if operation == "putRecord":
        assert (len(args) == 2)
        key = args[0]
        value = args[1]
        return putRecord(key, value)
    if operation == "getRecord":
        assert (len(args) == 1)
        key = args[0]
        return getRecord(key)
    return False


def putRecord(key, value):
    assert (_isOwner())
    Put(GetContext(), key, value)
    Notify(["putRecord", key, value])
    return True

def getRecord(key):
    return Get(GetContext(), key)

def _isOwner():
    return CheckWitness(Owner)
```


### 其他合约模板

| # | 智能合约                         |        语言      |   描述   |
| -----| ---------------------------------------- | ---------------- | ---------------- |
| 1| [Oep4](https://github.com/ONT-Avocados/python-template/blob/master/OEP4Sample/OEP4Sample_compiler2.0.py) |    Python     |    Oep4  |
| 2| [Oep5](https://github.com/ONT-Avocados/python-template/blob/master/OEP5Sample/OEP5Sample_compiler2.0.py) |  Python   |  Oep5   |
| 3| [Oep8](https://github.com/ONT-Avocados/python-template/blob/master/OEP8Sample/OEP8Sample_compiler2.0.py) |  Python   |  Oep8   |
| 4| [Invoke Oep4](https://github.com/ONT-Avocados/python-template/blob/master/Static_Call_Oep4/static_call_Oep4_compiler2.0.py) |  Python  | 在智能合约中调用 Oep4 方法 |
| 5| [Storage example](https://github.com/ONT-Avocados/python-template/blob/master/Storage_Example/storage_example_compiler2.0.py) |   Python  |   Storage 示例  |
| 6| [Struct example](https://github.com/ONT-Avocados/python-template/blob/master/Struct_Example/struct_example_compiler2.0.py) |   Python  | Struct 示例  |
| 7| [Invoke native in sc](https://github.com/ONT-Avocados/python-template/blob/master/NativeAssetInvoke/native_asset_invoke_compiler2.0.py) | Python | 在智能合约中调用 native |
| 8| [Migrate Contract](https://github.com/ONT-Avocados/python-template/blob/master/MigrateDestruct/migrate_destroyWithinContract_compiler2.0.py) | Python | 迁移合约 |
| 9| [Event test](https://github.com/ONT-Avocados/python-template/blob/master/EventTest/event_test_compiler2.0.py) | Python|   Event 测试  |
|10| [Smart contract libs](https://github.com/ONT-Avocados/python-template/tree/master/libs) | Python|  合约 libs    |

