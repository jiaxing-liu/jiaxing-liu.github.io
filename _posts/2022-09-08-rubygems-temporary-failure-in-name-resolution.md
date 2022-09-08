---
layout:     post
title:      "Rubygems: Temporary failure in name resolution"
subtitle:   "Gentoo"
date:       2022-09-08 12:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - Rubygems
    - Gentoo
---

## Problem

A "Temporary failure in name resolution" error appeared when I upgrade Gentoo linux system.

```bash
>>> Downloading 'https://rubygems.org/gems/psych-4.0.5.gem'
--2022-09-08 12:14:56--  https://rubygems.org/gems/psych-4.0.5.gem
Resolving rubygems.org... failed: Temporary failure in name resolution.
wget: unable to resolve host address ‘rubygems.org’
!!! Couldn't download 'psych-4.0.5.gem'. Aborting.
 * Fetch failed for 'dev-ruby/psych-4.0.5', Log file:
 *  '/var/tmp/portage/dev-ruby/psych-4.0.5/temp/build.log'

>>> Failed to emerge dev-ruby/psych-4.0.5, Log file:

>>>  '/var/tmp/portage/dev-ruby/psych-4.0.5/temp/build.log'

 * Messages for package dev-ruby/psych-4.0.5:

 * Fetch failed for 'dev-ruby/psych-4.0.5', Log file:
 *  '/var/tmp/portage/dev-ruby/psych-4.0.5/temp/build.log'
```

## Solutions: change gem source

```bash
$ gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
$ gem sources -l
https://gems.ruby-china.com
```

It is OK now.

Reference:
[https://gems.ruby-china.com/]

