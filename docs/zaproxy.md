# zaproxy

OWASP Zed攻击代理（ZAP）是世界上最受欢迎的免费安全工具之一，它使您可以自动查找应用程序中的安全漏洞。

## 安装

window 版本开箱即用，Linux 版本分情况，默认带UI启动，与 window版无异(java跨凭他);

另一种在服务器(无桌面)中运行(即以`-daemon`方式运行), 这种情况就需要通过API进行操作, 官方提供了详细的[API文档](https://www.zaproxy.org/docs/api/#introduction)。

做爬虫时需要使用到firefox浏览器，所以先安装一下firefox

```bash
yum intall -y firefox
```

默认情况下, 需要指定`api-key`和允许访问的地址才能正常使用API, 否则会提示没有权限, 下面的命令可以简易的去除各种限制启动API服务器,注意只能用于测试和学习。

```bash
./zap.sh -host 0.0.0.0 -port 8000 -daemon -config api.addrs.addr.name=.* -config api.addrs.addr.regex=true -config api.disablekey=true
```

## 使用

API文档 [API Reference](https://www.zaproxy.org/docs/api/#introduction)

 ZAP具有非常强大的API，通过API可以完成几乎所有界面可执行能的操作。这使开发人员可以自动在CI/CD管道中对应用程序进行渗透测试和安全性回归测试。



以下是ZAP提供的一些功能：

- 拦截代理(Intercepting Proxy)

- 主动和被动扫描仪(Active and Passive Scanners)

- 传统爬虫和AJAX爬虫(Traditional and Ajax Spiders)

- 暴力破解(Brute Force Scanner)

- 端口扫描(Port Scanner)

- 网络套接字(Web Sockets)

- 
  

The API documentation is divided into nine main sections.

- [**Introduction**](https://www.zaproxy.org/docs/api/#introduction) section contains introductory information of ZAP and installation guide to set up ZAP for testing.
- [**Exploring the App**](https://www.zaproxy.org/docs/api/#exploring-the-app) section contains examples on how to explore the web application.
- [**Attacking the App**](https://www.zaproxy.org/docs/api/#attacking-the-app) section contains examples on how to scan or attack a web application.
- [**Getting the Results**](https://www.zaproxy.org/docs/api/#getting-the-results) section contains examples on how to retrieve alerts and generate Reports from ZAP.
- [**Getting Authenticated**](https://www.zaproxy.org/docs/api/#getting-authenticated) section contains examples on how to authenticate the web application with ZAP.
- [**Advanced Settings**](https://www.zaproxy.org/docs/api/#advanced-settings) section contains advanced configurations on how to fine tune ZAP results.
- [**Contributions**](https://www.zaproxy.org/docs/api/#contributions-welcome) section contains guidelines and instructions on how to contribute to ZAP's documentation.
- [**API Catalogue**](https://www.zaproxy.org/docs/api/#api-catalogue) section contains OpenAPI definitions and auto generated code for ZAP APIs.
- [**Troubleshooting**](https://www.zaproxy.org/docs/api/#troubleshooting) section contains solutions for trouble shooting ZAP API related issues.

### API请求基础

ZAP API提供对ZAP大部分核心功能的访问，例如主动扫描和爬虫。 默认情况下，在守护程序模式和桌面模式下自动启用ZAP API。

> ZAP需要API密钥才能通过REST API执行特定的操作。 API密钥用于防止恶意站点访问ZAP API。 强烈建议您设置密钥，除非您在完全隔离的环境中使用ZAP。
> 
> 还要确保在调用ZAP核心未捆绑的功能时已安装了必要的加载项。 例如，如果收到与Ajax Spider调用有关的“ no_implementor错误”，则可能未安装Ajax Spider附加组件。

API允许使用`GET`和`POST`方法，并且响应可采用`JSON`，`XML`，`HTML`和`OTHER`（自定义格式，例如HAR）格式。 响应格式不同但是返回相同的信息。 根据用例，选择适当的格式。 例如，要生成易于阅读的报告，请使用HTML格式，使用基于XML/JSON的响应来快速解析结果。



#### 客户端SDK

| 语言      | 下载链接                                                                                                                                                         | 备注        |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| .NET    | [NuGet](https://www.nuget.org/packages/OWASPZAPDotNetAPI)                                                                                                    | 官方API     |
| Java    | [GitHub](https://github.com/zaproxy/zap-api-java/releases) [Maven Central](https://search.maven.org/search?q=g:org.zaproxy%20AND%20a:zap-clientapi&core=gav) | 官方API     |
| Node.js | [NPM](https://www.npmjs.org/package/zaproxy)                                                                                                                 | 官方API     |
| PHP     | [GitHub](https://github.com/yukisov/php-owasp-zap-v2) [Packagist](https://packagist.org/packages/zaproxy/php-owasp-zap-v2)                                   | 正在成为官方API |
| Python  | [PyPI](https://pypi.python.org/pypi/python-owasp-zap-v2.4)                                                                                                   | 官方API     |
| Ruby    | [GitHub](https://github.com/vpereira/owasp_zap)                                                                                                              | -         |

这里我们会使用Python API:

```bash
pip3 install python-owasp-zap-v2.4
```

### 浏览应用(Exploring the App)

为了向ZAP公开内容和功能以测试目标，应在执行任何扫描或攻击之前先探索应用程序。您越探索应用程序，结果将越准确。如果没有很好地探索该应用程序，则它将影响或减少ZAP可以发现的漏洞。

以下是使用ZAP浏览站点的一些选项。您可以组合使用多种方法来获得对应用程序的更完整覆盖。

传统爬虫：使用此方法可对Web应用程序中的HTML资源（超链接等）进行爬网。

Ajax爬虫：如果应用程序严重依赖Ajax调用，请使用此功能。

代理回归/单元测试：这是安全性回归测试的推荐方法。如果您已经有测试套件或单元测试，请使用这种方法来浏览应用程序。

OpenAPI/SOAP定义：如果您有定义明确的OpenAPI定义，请使用此方法。可以通过市场下载OpenAPI插件。

### 攻击应用(Attacking the App)

在开始扫描安全漏洞之前，应先浏览该应用程序。 以下部分提供了有关如何使用 **被动扫描** 和 **主动扫描** 在应用程序中查找安全漏洞的示例。

#### 被动扫描

通过ZAP代理或由爬虫发起的所有请求都将被被动扫描。 您不必手动启动被动扫描过程，默认情况下ZAP被动扫描所有HTTP和WebSocket消息（请求和响应）
发送到应用程序。被动扫描不会以任何方式更改请求或响应，因此可以安全使用。 这对于发现诸如缺少安全标头或缺少反CSRF令牌之类的问题很有好处，但对于寻找诸如XSS之类的需要发送恶意请求的漏洞则无济于事，因为这是 主动扫描 的工作。

被动扫描需要更多时间才能完成完整扫描。 爬取完成后，使用  [recordsToScan](https://www.zaproxy.org/docs/api/#pscanviewrecordstoscan) API获取剩余要扫描的记录数。 获取扫描结果的方式同主动扫描一致。

查看高级部分 [advanced section](https://www.zaproxy.org/docs/api/#passive-scan-settings) 以了解如何配置被动扫描的其他参数。

#### 主动扫描

主动扫描将通过使用对选定目标进行攻击来发现潜在漏洞。

##### 启动主动扫描

接口 [scan](https://www.zaproxy.org/docs/api/#ascanactionscan) 会启动一个主动扫描，对给定的URL或上下文进行扫描。 参数 `recurse` 用于扫描规定URL之下的URL。参数  `inScopeOnly` 用于约束扫描范围内的URL(会忽略已经声明的Context)。参数`scanPolicyName` 允许声明扫描策略(如果没有指定，使用默认扫描策略)。参数  `method` 和 `postData` allow to select a given request in conjunction with the given URL。

该接口会返回一个 `ID` 用于标识此次扫描。查看 [context](https://www.zaproxy.org/docs/api/#context-advanced), [scope](https://www.zaproxy.org/docs/api/#scope-advanced) 了解如何配置扫描策略。

##### 查看扫描状态

接口 [status](https://www.zaproxy.org/docs/api/#ascanviewstatus) 提供扫描进度的百分比。(需要指定扫描`ID`).

##### 查看扫描结果

扫描完成后，ZAP以警报的形式提供安全漏洞。接口 [alerts](https://www.zaproxy.org/docs/api/#alertviewalerts) 提供识别的警报信息。

警报摘要接口([alerts summary](https://www.zaproxy.org/docs/api/#alertviewalertssummary))可以获取按每个风险级别分组的警报数量，并可以选择按URL进行过滤。还可以使用核心模块生成摘要报告，使用  [htmlreport](https://www.zaproxy.org/docs/api/#coreotherhtmlreport) 、[jsonreport](https://www.zaproxy.org/docs/api/#coreotherjsonreport) 、 [xmlreport](https://www.zaproxy.org/docs/api/#coreotherxmlreport) 接口生成对应格式的摘要报告。 

##### 停止主动扫描

接口 [stop](https://www.zaproxy.org/docs/api/#ascanactionstop) 用于停止正在进行的主动扫描，可以使用接口 [stopAllScans](https://www.zaproxy.org/docs/api/#ascanactionstopallscans) 停止所有的主动扫描，接口 [pause](https://www.zaproxy.org/docs/api/#ascanactionpause) 可以暂停扫描。

应该注意的是，主动扫描只能发现某些类型的漏洞。 不能识别逻辑漏洞，例如错误的访问控制等。 除了主动扫描以发现所有类型的漏洞外，还应始终执行手动渗透测试。

## General Steps

The following are the general steps when configuring the application authentication with ZAP.

**Step 1. Define a context**

Contexts are a way of relating a set of URLs together. The URLs are defined as a set of regular expressions (regex). You should include the target application inside the context. The unwanted URLs such as the logout page, password change functionality should be added to the exclude in context section.

**Step 2. Set the authentication mechanism**

Choose the appropriate login mechanism for your application. If your application supports a simple form-based login, then choose the form-based authentication method. For complex login workflows, you can use the script-based login to define custom authentication workflows.

**Step 3. Define your auth parameters**

In general, you need to provide the settings on how to communicate to the authentication service of your application. In general, the settings would include the login URL and payload format (username & password). The required parameters will be different for different authentication methods.

**Step 4. Set relevant logged in/out indicators**

ZAP additionally needs hints to identify whether the application is authenticated or not. To verify the authentication status, ZAP supports logged in/out regexes. These are regex patterns that you should configure to match strings in the responses which indicate if the user is logged in or logged out.

**Step 5. Add a valid user and password**

Add a user account (an existing user in your application) with valid credentials in ZAP. You can create multiple users if your application exposes different functionality based on user roles. Additionally, you should also set valid session management when configuring the authentication for your application. Currently, ZAP supports cookie-based session management and HTTP authentication based session management.

**Step 6. Enable forced user mode (Optional)**

Now enable the ![](https://www.zaproxy.org/docs/desktop/images/fugue/forcedUserOff.png) "[Forced User Mode disabled - click to enable](https://www.zaproxy.org/docs/desktop/ui/tltoolbar/#--forced-user-mode-on--off)" button. Pressing this button will cause ZAP to resend the authentication request whenever it detects that the user is no longer logged in, ie by using the 'logged in' or 'logged out' indicator. But the forced user mode is ignored for scans that already have a user set.

In order for auth to work one of the indicators(logged in/out) needs to be set, however, ZAP will allow users to proceed without having to set them because there are other methods of setting them. However, if one isn't set, then the auth won't work.
