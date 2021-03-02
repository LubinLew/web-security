# Arachni

Arachni 是一个全面的、模块化的Ruby框架，它能够帮助渗透人员和网络管理人员测试Web应用的安全性。 Arachni是非常智能的，在安全审计过程中，它能够从每个HTTP响应中训练提升自己的检测能力。与其他扫描工具不同的是，它能够发现很多隐蔽较深的漏洞。

## 扫描站点

扫面的详细参数见`bin/arachni -h`结果.

```bash
bin/arachni http://www.test.com/
```

## 生成报表

扫描后会生成一个 `.afr` 文件, 使用`bin/arachni_reporter` 进行转换，导出文件的格式支持列表见 `bin/arachni_reporter --reporters-list`结果。

```bash
bin/arachni_reporter test.afr --reporter=json:outfile=test.json
```

## 使用UI进行操作

详细参数见`bin/arachni_web -h`结果.


