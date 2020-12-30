---
title: Jetsontx2使用笔记
author: Liang Chen
date: 2020-11-25 18:00:00 +0800
categories: [Notes]
tags: [Jetsontx2]
pin: false
toc: true
comments: false
math: true
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

1. How to set static ip in TX2 Board?

    > [https://www.cnblogs.com/shuimuqingyang/p/11668561.html](https://www.cnblogs.com/shuimuqingyang/p/11668561.html)

    ```shell
    # 01. check ip
    ifconfig
    # 02.
    sudo vim /etc/network/interfaces
    # 03. Add info as here
    auto eth0
    iface eth0 inet static
    address xxx.xx.xxx.xxx
    netmask 255.255.255.0
    dns-nameserver 8.8.8.8
    gateway xxx.xx.xxx.xxx 
    # 04. refresh ip
    sudo ip addr flush wlan0
    # 05. reboot network service
    sudo systemctl restart networking.service
    # 06. reboot TX2
    sudo reboot
    ```
