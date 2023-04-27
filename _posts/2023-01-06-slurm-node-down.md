---
layout:     post
title:      "Slurm node unexpectedly rebooted"
subtitle:   "Slurm"
date:       2023-01-06 17:00:00
author:     "JXLIU"
header-img: "img/post-bg-2015.jpg"
mathjax: true
tags:
    - Linux
    - Slurm
---

After the Slurm computing node is manually restarted, the management node will set the status of the computing node to DOWN

You can use the following command on the Slurm management node to restore the state of the computing node

```bash
scontrol update NodeName=nodename State=RESUME
```

Reference: [](https://actorsfit.com/a?ID=01750-99cbcb92-3e6d-4afb-ab08-13c367c68436)


Slurm Troubleshooting: Nodes stuck in CG status
```bash
scontrol update nodename=node006 state=down reason=cg
scontrol update nodename=node006 state=resume
```
Reference: []([https://actorsfit.com/a?ID=01750-99cbcb92-3e6d-4afb-ab08-13c367c68436](https://www.tigrillolinux.net/index.php/Slurm_Troubleshooting:_Nodes_stuck_in_CG_status))
