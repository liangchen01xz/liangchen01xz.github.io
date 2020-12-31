---
layout: post
title: vcs启动license服务
subtitle:
author: Liang Chen
date: 2020-12-17 18:00:00 +0800
tags: [Notes, eda]
catalog: true
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

参考文章

[https://www.bicner.com/846.html/comment-page-1](https://www.bicner.com/846.html/comment-page-1)

[https://blog.csdn.net/huayangshiboqi/article/details/89525723](https://blog.csdn.net/huayangshiboqi/article/details/89525723)

主要是两个问题，一个是释放27000端口；一个是使用正确的`license.dat`文件。

1. use `pgrep lmgrd` & `pgrep snpslmd` to check PID, and use `kill -9 PID` to kill it!

2. use `sudo su` to enter root.

3. use `/harddisk/disk1/eda/synopsys/scl/linux64/bin/lmgrd -c /harddisk/disk1/eda/synopsys/license/synopsys.dat [-l /harddisk/disk1/eda/synopsys/license/debug.log]` to launch license server.

