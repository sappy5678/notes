---
title: "dpkg: error processing archive *"
date: 2017-08-19 12:39:58
tags: [ubuntu,apt,dpkg]
categories: [ubuntu,apt,dpkg]
---

# 前提
在更新時遇到
```
dpkg: error processing archive /var/cache/apt/archives/python-lldb-6.0_1%3a6.0~svn310966-1~exp1_amd64.deb (--unpack): trying to overwrite '/usr/lib/python2.7/dist-packages/lldb', which is also in package python-lldb-4.0 1:4.0-1ubuntu1
```

看到網路上的[解法](https://github.com/dnschneid/crouton/issues/2631)
```
sudo dpkg --force-all -i /var/cache/apt/archives/linux-libc-dev_3.13.0-88.135_armhf.deb
```

於是稍微修改一下，變成
```
sudo dpkg --force-all -i /var/cache/apt/archives/python-lldb-6.0_1%3a6.0~svn310966-1~exp1_amd64.deb
```

就大功告成了
