---
layout:     post
title:      "Frequently used tools in Linux"
subtitle:   "Easies, Better, Happier"
date:       2023-10-15 16:00:00
author:     "JXLIU"
header-img: "img/sun1_2023.jpg"
mathjax: true
tags:
    - Linux
    - tools
---

## Frequently used tools on Linux
1. screen
   Frequently used command
   ```bash
   screen -S session_name # Create a new session
   screen -r session_name # return to an existing session
   ```
   Create multiple terminal session and detach from it after executing a command.
   ```bash
   #!/bin/bash
   for i in {10..68..2}, do
       screen -d -m -S "$i" bash -c "cd contourbin_$i;./run.sh;exec sh"
   Done
   ```
   Another similar tool is `tmux`.
2. Gnuplot

[gnuplot Scripts](https://www.bwcx.top/2024/0710/24/gnuplot/)