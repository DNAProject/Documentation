<h1 align="center">智能合约存取区块链数据</h1>

## 概述

某些特定类型的 ```dApp``` 可能有往链上存取数据的需求，本文档介绍了如何在链上进行数据存取操作。

链上数据的存储主要在于通过智能合约进行操作，以下是一个智能合约的例子,请在 ```SmartX``` 中调试，请参考 [调试指南](00-dapp_development)

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
