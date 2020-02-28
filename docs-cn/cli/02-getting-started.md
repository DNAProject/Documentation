
当你安装好DNA区块链的客户端后，便可以使用以下命令进行一些常用操作。

**查看客户端版本**

```shell
./dnaNode -v
```

**查看帮助信息**

```shell
./dnaNode help
```

**连接到主网**

```shell
./dnaNode --config=config.json
```

**启动测试模式**

在测试模式下，节点独立运行，

```shell
./dnaNode --testmode
```

**第一次启动测试模式之前，需要创建钱包文件。通过如下命令创建账户文件。

```shell
./dnaNode account add -d
```