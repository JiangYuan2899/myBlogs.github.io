---
layout: post
title: oracle服务器上导出dmp
category: other
tags: [other]
---

1.第一步oracle服务器lunix系统通过ftp连到上面；IP地址通过tns名来确认：例如

```
SJZHDB =
  (DESCRIPTION =
    (ADDRESS = (PROTOCOL = TCP)(HOST = 10.85.69.28)(PORT = 1521))
    (CONNECT_DATA =
      (SERVER = DEDICATED)
      (SERVICE_NAME = SJZHDB)
    )
  )
```

2.导出语句

>exp hy_rsrc/hy_rsrc@sjzhdb file=/home/Oracle/expdat.dmp

注意@后面名字根据SERVICE_NAME来确定。默认导出dmp名字expdat.dmp 再敲一下回车就可以了。。


3.下载服务器到本机就可以了