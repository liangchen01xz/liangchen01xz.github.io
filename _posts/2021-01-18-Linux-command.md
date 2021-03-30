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
```

```bash
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
```

```bash
scp lchen@172.xx.xxx.xxx:/home/lchen/work/backend/innovus/script.tcl cliang935@172.xx.xxx.xxx:/home/cliang935/work
```

```bash
du -hs ./*
```

```bash
free -hm 
```

```bash
top
htop
```

```bash
ln -s /harddisk/disk2/lchen_test/ /home/lchen/
```
