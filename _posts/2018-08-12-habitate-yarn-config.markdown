---
layout: layout
title: "Overview of chimera virtual machine"
date: 2018-08-12 18:00:00 -0700
categories: habitat yarn
---

[Home](/) [Personal](/about/)

Working on setting up habitat to deploy the neuralknight project.

Error line outputs
`Error: ENOENT: no such file or directory, open '/usr/local/share/.yarnrc'`

First fix attempted

`{PROJECT_DIR}/.studiorc`
```sh
export YARN_DISABLE_SELF_UPDATE_CHECK="true"
```
