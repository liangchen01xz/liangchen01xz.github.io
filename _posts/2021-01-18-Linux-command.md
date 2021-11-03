---
layout: post
title: Linux基操
subtitle:
author: Liang Chen
date: 2021-01-18 18:00:00 +0800
tags: [Notes, Linux]
catalog: true
---

> Reference: [The Art of Command Line](https://github.com/jlevy/the-art-of-command-line)

`<ctrl-r>` `<ctrl-w>` `<ctrl-u>` `<ctrl-k>` `<ctrl-a>` `<ctrl-e>` `<ctrl-l>` `<alt-b>` `<alt-f>` `<alt-.>`

```bash
su - root

su - lchen
sudo su
```

```bash
useradd -m username
passwd username
```

```bash
vncserver -geometry 1920x1080 :5
vncserver -depth 8 :5

vncserver :5

ps -ef | grep Xvnc
ps -aux | grep Xvnc

vncserver -kill :5
```

```bash
who
who -H
who -q
```

```bash
chown lchen file
chown -R lchen folder

chgrp lchen file
chgrp -R lchen folder

chmod 777 file
u:user g:group o:others a:all
r:4    w:2     x:1
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
locate

which

find ./ -name "*.py"
find ./ -name "*.py" -type d

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

Reference: [https://cloud.tencent.com/developer/article/1626785](https://cloud.tencent.com/developer/article/1626785)
```bash
sudo dd if=/dev/zero of=/swapfile bs=1024 count=33554432 # bs*count=32G
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
swapon --show
free -hm
sudo sed -i '$a /swapfile swap swap defaults 0 0' /etc/fstab

cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=60 # server machine value
sudo vi /etc/sysctl.conf
append 'vm.swappiness=60'

sudo swapoff -v /swapfile
sudo sed -i '\/swapfile\ swap\ swap\ defaults\ 0\ 0/d' /etc/fstab
sudo rm /swapfile
```

```bash
top
htop

htop -u lchen
pgrep -u lchen -l
pgrep -u lchen vivado
pgrep -u lchen | sudo xargs kill -9
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
cat -n test.v

sed -i '$a newline' test.v
sed -i '$i newline' test.v
sed -i '1a newline' test.v
sed -i '1i newline' test.v
sed -i 'na newline' test.v # n: line number
sed -i 'ni newline' test.v 

sed -i '$d' test.v
sed -i 'nd' test.v
sed -i '1,4d' test.v

sed -i '1,$s/old/new/g' test.v

sed -i '2,4c 2number\n3number\n4number' test.v

sed -i '/match/d' test.v
sed -i '/match/p' test.v
sed -i '/\<match\>/d' test.v
sed -i '/\<match\>/p' test.v
sed -i '/match/{s/old/new/g}' test.v
sed -i '/\<match\>/{s/old/new/g}' test.v

sed -i '/match/{s/old/new/g;p}' test.v
sed -i '/match/{s/old/new/g;p;q}' test.v

sed -e '$a newline' test.v > new.v
sed -e '$i newline' test.v
sed -e '1a newline' test.v
sed -e '1i newline' test.v
sed -e 'na newline' test.v # n: line number
sed -e 'ni newline' test.v

sed -e '$d' test.v
sed -e 'nd' test.v
sed -e '1,4d' test.v

sed -e '1,$s/old/new/g' test.v

sed -e '2,4c 2number\n3number\n4number' test.v

cat -n test.v | sed -n '/\<match\>/p'
cat -n test.v | sed -n '/match/p'

sed -e '/match/d' test.v
sed -e '/match/p' test.v
sed -e '/\<match\>/d' test.v
sed -e '/\<match\>/p' test.v
sed -e '/match/{s/old/new/g}' test.v
sed -e '/\<match\>/{s/old/new/g}' test.v

sed -e '/match/{s/old/new/g;p}' test.v
sed -e '/match/{s/old/new/g;p;q}' test.v
```
