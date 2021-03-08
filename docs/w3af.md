# w3af

## 安装

w3af 仍然使用python2, 所以安装比较麻烦

```bash
yum install -y gcc npm python-devel libxml2-devel libxslt-devel

## 安装python 2 
wget https://bootstrap.pypa.io/pip/2.7/get-pip.py
python get-pip.py

pip install --upgrade setuptools
pip install --upgrade six==1.13.0
pip install --ignore-installed PyYAML==3.12
pip install --ignore-installed requests==2.22.0

 pip install pyClamd==0.4.0 \
    PyGithub==1.21.0 \
    GitPython==2.1.15 \
    pybloomfiltermmap==0.3.14 \
    phply==0.9.1 \
    nltk==3.0.1 \
    chardet==3.0.4 \
    tblib==0.2.0 \
    pdfminer==20140328 \
    futures==3.2.0 \
    pyOpenSSL==18.0.0 \
    ndg-httpsclient==0.4.0 \
    pyasn1==0.4.2 \
    scapy==2.4.0 \
    guess-language==0.2 \
    cluster==1.1.1b3 \
    msgpack==0.5.6 \
    python-ntlm==1.0.1 \
    halberd==0.2.4 \
    darts.util.lru==0.5 \
    Jinja2==2.10 \
    vulndb==0.1.1 \
    markdown==2.6.1 \
    psutil==5.4.8 \
    ds-store==1.1.2 \
    termcolor==1.1.0 \
    mitmproxy==0.13 \
    ruamel.ordereddict==0.4.8 \
    Flask==0.10.1 \
    tldextract==1.7.2 \
    pebble==4.3.8 \
    acora==2.1 \
    esmre==0.3.1 \
    diff-match-patch==20121119 \
    bravado-core==5.15.0 \
    lz4==1.1.0 \
    vulners==1.3.0 \
    ipaddresses==0.0.2 \
    subprocess32==3.5.4 \
    lxml==3.4.4 \
    Werkzeug==0.16.1

npm install -g retire@2.0.3
```

## 使用

支持UI、命令行 和 API 模式， 这里使用 API模式

```bash
./w3af_api 0.0.0.0:5000 --no-ssl  ## 仅用于测试
```

服务启动后, 使用client 访问, 注意官方提供的API是 Python 3 的,  `pip install --upgrade w3af-api-client` 无法正常安装，

```bash
## 安装客户端库
git clone https://github.com/andresriancho/w3af-api-client.git
cd w3af-api-client
python3 setup.py install
```

使用文档 [w3af REST API Introduction](http://docs.w3af.org/en/latest/api/index.html)

```python
#!/bin/env python3

from w3af_api_client import Connection, Scan

# Connect to the REST API and get it's version
conn = Connection('http://127.0.0.1:5000/')
print(conn.get_version())

# Define the target and configuration
scan_profile = file('profile.pw3af').read()
target_urls = ['http://example.target']

scan = Scan(conn)
scan.start(scan_profile, target_urls)

# Wait some time for the scan to start and then
scan.get_urls()
scan.get_log()
scan.get_findings()
```

[profile](https://github.com/andresriancho/w3af-api-client/blob/master/ci/constants.py) 文件例子如下:

```ini
[profile]
description = sqli
name = sqli
[crawl.web_spider]
only_forward = True
follow_regex = .*
ignore_regex =
[audit.sqli]
[outputconsole]
verbose = True
[target]
target = http://127.0.0.1:8000/audit/sql_injection/
[misc-settings]
fuzz_cookies = False
fuzz_form_files = True
fuzz_url_filenames = False
fuzz_url_parts = False
fuzzed_files_extension = gif
fuzzable_headers =
form_fuzzing_mode = tmb
stop_on_first_exception = False
max_discovery_time = 120
interface = wlan1
local_ip_address = 10.1.2.24
non_targets =
msf_location = /opt/metasploit3/bin/
[http-settings]
timeout = 0
headers_file =
basic_auth_user =
basic_auth_passwd =
basic_auth_domain =
ntlm_auth_domain =
ntlm_auth_user =
ntlm_auth_passwd =
ntlm_auth_url =
cookie_jar_file =
```
