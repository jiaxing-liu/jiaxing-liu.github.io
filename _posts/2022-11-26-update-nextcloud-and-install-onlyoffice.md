---
layout:     post
title:      "Upgrade nextcloud and install onlyoffice"
subtitle:   "docker"
date:       2022-11-26 18:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - nextcloud
    - docker
    - linux
---

## 备份数据
至少备份　`/var/www/html/data` 和　`/var/www/html/config` 文件夹，其余文件可不备份

## 升级　nextcloud 

修改　docker-compose.yml 中　nextcloud 版本，然后 `docker-compose up`，等待升级完成即可。

注意不能跨版本升级，不能　downgrade。
    如果不小心跨版本升级，需要从[官网](https://nextcloud.com/changelog/) 下载升级前　nextcloud 安装包，解压后覆盖掉 `/var/www/html/config`，`/var/www/html/data` 之外的文件夹，之后重启　docker，一级一级升级。 


## 安装　onlyoffice

### 使用　docker 安装　onlyoffice server

```bash
docker run --name onlyoffice-alone -i -t -d -p 8081:80 -p 8443:443 \
    -e LETS_ENCRYPT_DOMAIN=onlyoffice.hlug.cn -e LETS_ENCRYPT_MAIL=liu_jiaxing@hust.edu.cn \
    -v /home/lug/onlyoffice-alone/logs:/var/log/onlyoffice  \
    -v /home/lug/onlyoffice-alone/data:/var/www/onlyoffice/Data \
    -v /home/lug/onlyoffice-alone/lib:/var/lib/onlyoffice  \
    -v /home/lug/onlyoffice-alone/db:/var/log/postgresql  \
    onlyoffice/documentserver
```
暂时没有配置　ssl。

### Integrate onlyoffice with nextcloud

使用管理员帐号在　nextcloud app store 中安装　onlyoffice，配置　onlyoffice server address : http://xxx:8081。

进入　onlyoffice docker 查看　`JWT_secret`　和 `JWT_header`，该信息位于 `/etc/onlyoffice/documentserver/local.json` 中
```json
     "token": {
        "enable": {
        "request": {
            "inbox": true,
            "outbox": true
          },
          "browser": true
        },
        "inbox": {
          "header": "eeeeeeeeeeeee",
          "inBody": false
        },
        "outbox": {
          "header": "eeeeeeeeeeeee",
          "inBody": false
        }
      },
      "secret": {
        "inbox": {
          "string": "xxxxxxxxxxxxxxxxxxxx"
        },
        "outbox": {
          "string": "xxxxxxxxxxxxxxxxxxxx"
        },
        "session": {
          "string": "xxxxxxxxxxxxxxxxxxxx"
        }
      }
```

在　nextcloud 配置文件中加上如下内容

```php
  'onlyoffice' => array (
    'verify_peer_off' => true,
    "jwt_secret" => "sUaFx6t8abRmW9SqSMRc",
    "jwt_header" => "Authorization"
  ),
```
`'verify_peer_off' => true,` 是为了支持　`http`

重启　nextcloud，并配置　onlyoffice。

References:
[1] https://api.onlyoffice.com/editors/nextcloud
[2] https://github.com/ONLYOFFICE/onlyoffice-nextcloud
[3] https://helpcenter.onlyoffice.com/installation/docs-community-install-docker.aspx?_ga=2.51711023.782359554.1594636128-1157782750.1587541027
