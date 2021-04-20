---
layout: post
title: Linux基操
subtitle:
author: Liang Chen
date: 2021-01-18 18:00:00 +0800
tags: [Notes, Linux]
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

> Reference: [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)

```bash
find ./ -name "*.py"
find ./ -name "*.py" -type d
```

```bash
locate
```

```bash
grep -r "*.py" ./ -I
```

```bash
scp -r lchen@172.xx.xxx.xxx:/home/lchen/work/backend cliang935@172.xx.xxx.xxx:/home/cliang935/work
scp lchen@172.xx.xxx.xxx:/home/lchen/work/backend/innovus/script.tcl cliang935@172.xx.xxx.xxx:/home/cliang935/work
```

```bash
df -hl
df -h
du -hs
du -hs ./*
du -h
```

```bash
free -hm 
```

Reference: https://www.shuzhiduo.com/A/LPdoe4VyJ3/
```bash
sync
echo 3 > /proc/sys/vm/drop_caches or sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'
echo 0 > /proc/sys/vm/drop_caches
sudo swapoff -a
sudo swapon -a
```

```bash
top
htop
```

```bash
ln -s /harddisk/disk2/work_lchen/ /home/lchen/
```

```bash
tar -czvf test.tar.gz test
tar -czvf /home/lchen/work/test.tar.gz test
tar -czvf test.tar.gz --exclude=test/log --exclude=test/logv test

tar -tzvf test.tar.gz

tar -xzvf test.tar.gz
tar -xzvf test.tar.gz -C /home/lchen/work
```
