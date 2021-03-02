# zaproxy

window 版本开箱即用，Linux 版本分情况，默认带UI启动，与 window版无异(java跨凭他);

另一种在服务器(无桌面)中运行(即以`-daemon`方式运行), 这种情况就需要通过API进行操作, 官方提供了详细的[API文档](https://www.zaproxy.org/docs/api/#introduction)。

默认情况下, 需要指定`api-key`和允许访问的地址才能正常使用API, 否则会提示没有权限, 下面的命令可以简易的去除各种限制启动API服务器,注意只能用于测试和学习。

```bash
./zap.sh -host 0.0.0.0 -port 8000 -daemon -config api.addrs.addr.name=.* -config api.addrs.addr.regex=true -config api.disablekey=true
```




