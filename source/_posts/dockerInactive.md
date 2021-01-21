---
title: snap docker is inactive
date: 2021-01-21 16:18
tags: -Fail to start snap docker
---

## snap start docker 啟動後還是 inactive 的怪問題.



前一陣子,經常只要重新開機後,就會導致 docker 無法正常運行,常常需要照著(這篇)[https://askubuntu.com/questions/907110/docker-snap-cannot-connect-to-the-docker-daemon-is-the-docker-daemon-running-o]重新安裝. 又要多花 5~10 Mins 的時間.
```
sudo snap remove docker --purge
sudo snap install docker
```

後來找到這篇文章(StackOverflow)[https://stackoverflow.com/questions/63613629/docker-via-snap-cannot-connect-to-the-docker-daemon-on-a-year-old-installation] 大致上是先透過 
```
snap logs docker
``` 
尋找錯誤訊息,然後就看到 docker.dockerd: Failed to start docker daemon: pid file found ensure docker is not running or delete /var/snap/docker/471/run/docker.pid, 後來把 pid 移除後,重新 sudo snap start docker 就可以正常運行了.

