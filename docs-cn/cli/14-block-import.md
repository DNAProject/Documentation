<h1 align="center">Import命令介绍</h1>

## 1. 区块导入

DNA区块链客户端 `DNA-CLI` 提供了区块导入模块，能够将存有区块数据的压缩文件导入到本地节点，可以在命令行中通过 `import` 命令使用。

- `--datadir`：指定区块数据存储目录
- `--config`：指定当前节点创世区块配置文件的路径（默认使用主网的创世块配置）
- `--disableeventlog`：关闭导入区块时生成合约日志功能
- `--endheight`：指定导入区块数据的目标区块高度（默认导入所有的区块）
- `--importfile`：指定导入文件的路径（默认为 `./DNABlocks.dat`）

!> 出于安全考虑，请确保导入的区块数据文件的来源是可信的。

```shell
./dnaNode import --importfile=./Blocks.dat
```

你也可以通过 `--help` 选项获取帮助信息。

```shell
dnaNode import --help
```


## 2. 区块导出


DNA区块链客户端 `DNA-CLI` 提供了区块导出模块，能够将本地节点的区块数据导出到一个压缩文件之中，可以在命令行中通过 `export` 命令使用。

- `--exportfile`：指定导出文件的路径（默认为 `./DNABlocks.dat`）
- `--startheight`：指定导出区块的起始高度（默认为 `0`）
- `--endheight`：指定导出区块的终止高度（默认值为 `0`，表示导出所有区块）
- `--speed`：指定导出速度，`h` 表示快速导出，`m` 表示正常导出，`l` 表示慢速导出（默认值为`m`）

```shell
$ dnaNode export
Start export.
Block(3653/3652) [====================================================================] 100%   10s
Export blocks successfully.
StartBlockHeight:0
EndBlockHeight:3652
Export file:./DNABlocks_0_3652.dat
```

你也可以通过 `--help` 选项获取帮助信息。

```shell
dnaNode export --help
```
