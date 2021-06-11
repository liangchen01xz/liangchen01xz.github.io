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

`<ctrl-r>` `<ctrl-w>` `<ctrl-u>` `<ctrl-k>` `<ctrl-a>` `<ctrl-e>` `<ctrl-l>` `<alt-b>` `<alt-f>` `<alt-.>`

```shell
su - root

su - lchen
sudo su
```



```bash
chown lchen file
chown -R lchen folder

chgrp lchen file
chgrp -R lchen folder

chmod 777 file
u:user g:group o:others a:all
chmod a+rwx file
chmod u+rwx file
chmod g+rwx file
chmod o+rwx file
chmod u-w file
chmod g-w file
chmod o-w file
chmod u=rw, go= file
chmod -R u+rw, go-w folder
```

```bash
find ./ -name "*.py"
find ./ -name "*.py" -type d
```

```bash
locate
```

```bash
which
```

```bash
grep -r "*.py" ./ -I
```

```bash
scp -r lchen@172.xx.xxx.xxx:/home/lchen/work/backend liangchen01xz@172.xx.xxx.xxx:/home/liangchen01xz/work
scp lchen@172.xx.xxx.xxx:/home/lchen/work/backend/innovus/script.tcl liangchen01xz@172.xx.xxx.xxx:/home/liangchen01xz/work
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

Reference: [https://www.shuzhiduo.com/A/LPdoe4VyJ3/](https://www.shuzhiduo.com/A/LPdoe4VyJ3/)
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
// zip
tar -czvf test.tar.gz test
tar -czvf /home/lchen/work/test.tar.gz test
tar -czvf test.tar.gz --exclude=test/log --exclude=test/logv test

// list
tar -tzvf test.tar.gz

// unzip
tar -xzvf test.tar.gz
tar -xzvf test.tar.gz -C /home/lchen/work
```

```bash
awk
sed
grep
```
