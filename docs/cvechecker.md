# cvechecker



## 安装

> [Installation · sjvermeu/cvechecker Wiki](https://github.com/sjvermeu/cvechecker/wiki/Installation)

```bash
yum install -y sqlite-devel libbsd-devel
 autoreconf --force --install
./configure --enable-sqlite3
make
make install
 make postinstall
```

## 使用

```bash
find / -type f -perm -o+x > scanlist.txt
echo /proc/version >> scanlist.txt

cvechecker -b scanlist.txt

cvechecker -r
```


