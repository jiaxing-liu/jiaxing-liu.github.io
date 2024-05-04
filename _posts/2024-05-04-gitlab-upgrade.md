---
layout:     post
title:      "Self-hosted GitLab upgrade"
subtitle:   "docker"
date:       2024-01-11 16:00:00
author:     "JXLIU"
header-img: "img/sun1_2023.jpg"
mathjax: true
tags:
    - Gitlab
    - docker
---

## 背景
自建 gitlab server 最近频繁遇到 `500 error`，网上查询到升级可能可以解决问题。
目前版本 `gitlab-ce:13.12.12-ce.0`

## Upgrade path

了解后发现，一般来说 `gitlab server` 更新的越快越好，一般来说1周-1月至少更新一次，否则可能会出现问题。

以下是我的更新路径

`13.12.12` -> `13.12.15` -> `14.0.12`-> `14.9.0` -> `14.10.5` -> 
`15.0.5` -> `15.1.6` -> `15.4.6` -> `15.11.13` ->
`16.0.8` -> `16.1.6`-> `16.2.9` -> `16.3.7` > `16.7.z` -> `16.11.z`

升级到 `13.12.15`，便解决了频繁出现的 `500 error`，继续后续更新时，时不时又出现了 `500 error`

## 遇到的问题

### `13.12.15` -> `14.0.12` 
过程中出现了以下错误

```bash
Recipe Compile Error in /opt/gitlab/embedded/cookbooks/cache/cookbooks/gitlab-ee/recipes/default.rb
```
借鉴 [GItlab](https://gitlab.com/gitlab-org/omnibus-gitlab/-/issues/3977) 上的回答，解决了问题。
`docker-compose up` 前删除 `gitlab.rb` 文件，成功启动后，再把原来的配置写入 `gitlab.rb` 文件

### `14.0.12` -> `14.9.0`

遇到以下错误
```bash
| Chef Infra Client failed. 13 resources updated in 55 seconds
gitlab | There was an error running gitlab-ctl reconfigure:
gitlab |
gitlab | rails_migration[gitlab-rails] (gitlab::database_migrations line 51) had an error: Mixlib::ShellOut::ShellCommandFailed: bash[migrate gitlab-rails database] (/opt/gitlab/embedded/cookbooks/cache/cookbooks/gitlab/resources/rails_migration.rb line 16) had an error: Mixlib::ShellOut::ShellCommandFailed: Expected process to exit with [0], but received '1'
gitlab | ---- Begin output of "bash"  "/tmp/chef-script20240504-31-1dn2mnj" ----
gitlab | STDOUT: rake aborted!
gitlab | StandardError: An error has occurred, all later migrations canceled:
gitlab |
gitlab | Expected batched background migration for the given configuration to be marked as 'finished', but it is 'active':     {:job_class_name=>"CopyColumnUsingBackgroundMigrationJob", :table_name=>"ci_stages", :column_name=>"id", :job_arguments=>[["id"], ["id_convert_to_bigint"]]}
gitlab |
gitlab | Finalize it manualy by running
gitlab |
gitlab |        sudo gitlab-rake gitlab:background_migrations:finalize[CopyColumnUsingBackgroundMigrationJob,ci_stages,id,'[["id"]\, ["id_convert_to_bigint"]]']
```

根据 [Gitlab](https://docs.gitlab.com/ee/update/versions/gitlab_14_changes.html#1490) 提示

1. 启动 `gitlab-rails console`

```bash
docker exec -it <container-id> gitlab-rails console
```
2. 执行以下命令

```bash
Gitlab::Database::BackgroundMigrationJob.pending.where(class_name: "ResetDuplicateCiRunnersTokenValuesOnProjects").find_each do |job|
  puts Gitlab::Database::BackgroundMigrationJob.mark_all_as_succeeded("ResetDuplicateCiRunnersTokenValuesOnProjects", job.arguments)
end
```

### 

```bash
Caused by:
gitlab | PG::CheckViolation: ERROR:  check constraint "check_70f294ef54" is violated by some row
```
