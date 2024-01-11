---
layout:     post
title:      "Self-hosted GitLab pages setting"
subtitle:   "docker"
date:       2024-01-11 16:00:00
author:     "JXLIU"
header-img: "img/sun1_2023.jpg"
mathjax: true
tags:
    - Gitlab
    - docker
---


## 修改 `gitlab.rb` 文件

```bash
 pages_external_url "http://pages.bwcx.top:8980/"
 gitlab_pages['enable'] = true
 gitlab_pages['inplace_chroot'] = true
 gitlab_pages['env'] = {'TMPDIR' => '/opt/tmp/gitlab-pages'} ## 这个应该是不必要的
 gitlab_pages['access_control'] = true
```

## resolve

将系统文件 `/etc/resolv.conf` 拷贝至容器内 `/var/opt/gitlab/gitlab-rails/shared/pages/etc`

## port exposition

`docker-compose.yml` 文件中增加 `8980:8980` 端口的暴露。
