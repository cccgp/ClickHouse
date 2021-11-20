# ClickHouse测试

## 功能测试

功能测试是最简单和最方便使用的测试。大多数ClickHouse功能都可以通过功能测试进行测试，而且可以通过这种方式保证后续修改的代码仍然保持正确的功能。

每个功能测试将一个或多个查询发送到正在运行的ClickHouse服务器，并将运行结果与参考结果进行比较。

测试位于`queries`目录，该目录有两个子目录： `stateless` and `stateful`，`stateless`中的测试能够在不需要任何预加载测试数据情况下运行查询 - 它们经常在测试方法内部动态地生成小型合成数据集。`stateful`中的测试需要Yandex的预加载测试数据才能运行，这些测试数据可供公众使用。

每一个测试分为两种类型：`.sql` and `.sh`，`.sql`测试是一个简单的SQL脚本，通过管道传输到`clickhouse client--multiquery--testmode`，`.sh`测试是一个自己运行的脚本，SQL测试一般优于`.sh`测试，仅仅当你必须测试一些通过纯SQL无法测试的功能，才使用`.sh`测试，例如将一些输入数据传输到`clickhouse-client` 或者测试 `clickhouse-local`。

### 本地运行测试

在本地启动ClickHouse服务器，侦听默认端口(9000)。例如，为了运行测试`01428_HASH_SET_NAN_KEY`，首先更改文件夹，然后运行如下命令：

```
PATH=$PATH:<path to clickhouse-client> tests/clickhouse-test 01428_hash_set_nan_key
```

对于更多选项，请参考`test/clickhouse-test--help`。你可以简单地运行所有测试或通过测试名称的子串筛选测试子集：`./clickhouse-test subing`。还可以选择并行或随机顺序运行测试。
