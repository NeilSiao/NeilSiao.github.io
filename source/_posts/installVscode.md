---
title: Install VSCode in ubuntu
date: 2020-12-23 20:07
tags: -ubuntu dpkg ide
---

## how to install VSCode in ubuntu

(VSCode Official website)[https://code.visualstudio.com/docs/?dv=linux64_deb] 
1. Download VSCode deb package 
2. Open it by ubuntu software store
3. finished!

### Option.1
- If it shows unsupported file then use another way, open a command line to install
type command below.
```
sudo dpkg -i vscode.deb
```

### Option.2 
move your downloaded file to home directory(~), because Ubuntu software is unable to reach it in /tmp directory. looks like a bug.







